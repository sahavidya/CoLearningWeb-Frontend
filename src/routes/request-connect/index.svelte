<script context="module">

export async function preload({ query }) {
        console.log("pre-load");
        console.log(query);
		const {mentorId} = query;
		return {mentorId};
	}

</script>

<script>
    import { goto, stores } from "@sapper/app";
    import * as api from 'api.js';
    const { session, page } = stores();
    let connectionAs;
    let timeCommitment;
    let personalNoteRequest = "";
    let skillFluency = 0;
    let userData;
    let mentorSkillsArr;
    let menteeskill;
    let notes;
    export let mentorId;
    console.log($page.query);
    const connectionOptions = [
        { id : 'learner', text: 'As a student'},
        { id : 'colearner', text: 'As a co-learner'}
    ];
   
    async function getData() {
		console.log($session.user.access_token);
        //const response = await api.get(`users/${$page.query}`, $session.user.access_token);
        console.log("MentorId Query:" + mentorId); 
        const response = await api.get(`users/${mentorId}`, $session.user.access_token);
		console.log(response);
        userData = response;
        mentorSkillsArr = userData.skills.split("|");
	}
    getData();
    
    async function submit(event) {
    
        let notes;
        //notes: skillname, skillfluency, timecommitment, connectiontype, personalnoterequest
        notes = menteeskill + "|" + skillFluency + "|" +  timeCommitment + "|" + connectionAs.id + "|" +  personalNoteRequest;
;  
        console.log("Mentor Id:" + userData.id);
        console.log("Mentee Id:" + $session.userid);
        console.log("Notes:" + notes);

		const response = await api.post(`mentorship_relation/send_request/`,
		 {
            "mentor_id": userData.id,
            "mentee_id": $session.userid,
            "end_date": 1617235199, // TBD : end date is UNIX TIME STAMP and has been hardcoded to 31/Jan/2021
            "notes": notes,
		 }, $session.user.access_token );

		//errors = response.errors;
		//if (response.user) $session.user = response.user;
        //inProgress = false
        goto('/Home');
	}
</script>

<style>
.request-text{
    position: relative;
    top: 25%;
}
.languages{
    float: right;
}
</style>

<svelte:head>
	<title>Request to Connect</title>
</svelte:head>

{#if userData !== undefined}
<div class="request-connect-page">
    <div class="container page">
		<div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Request to Connect</h1>
                <hr />
                <form on:submit|preventDefault={submit}>
                    <div class="media">
                        <div class="media-left">
                            <img class="media-object img-circle" src="images/temp.jpg" alt="Profile" width="50" height="50">
                        </div>
                        <div class="media-body">
                            <h4 class="media-heading request-text">Connection request to {userData.name}}</h4>
                        </div>
                    </div>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                Learning domain:
                            </div>
                            <div class="col-sm-8">
                               
                                <select class="form-control form-control-md" bind:value={menteeskill}>
                                    {#each mentorSkillsArr as skill}
                                        <option value={skill}>
                                            {skill}
                                        </option>
                                    {/each}
                                </select>
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                Connection:
                            </div>
                            <div class="col-sm-8">
                                <select class="form-control form-control-md" bind:value={connectionAs}>
                                    {#each connectionOptions as option}
                                        <option value={option}>
                                            {option.text}
                                        </option>
                                    {/each}
                                </select>
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-6">
                                Communication language
                            </div>
                            <div class="col-sm-6">
                                <span class="languages">English, Hindi, Marathi</span>
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                Current City:
                            </div>
                            <div class="col-sm-8">
                                
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-8">
                                Fluency in {`language`} (Self rated):
                            </div>
                            <div class="col-sm-4">
                                <input type="number" class="form-control" min=0 max=10 bind:value={skillFluency}>
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-8">
                                Time you will spend to learn this topic daily:
                            </div>
                            <div class="col-sm-4">
                                <input type="text" class="form-control" bind:value={timeCommitment}>
                            </div>
                        </div>
                    </fieldset>
                    <p>
                        Write down something about yourself.
                    </p>
                    <fieldset class="form-group">
                        <textarea class="form-control" rows="3" bind:value={personalNoteRequest}></textarea>
                    </fieldset>
                    <button class="btn btn-md btn-primary pull-xs-right">
                        Send request
                    </button>
                </form>
            </div>
		</div>
    </div>
</div>
{/if}