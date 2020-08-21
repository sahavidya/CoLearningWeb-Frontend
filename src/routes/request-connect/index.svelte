<script>
    import { stores } from "@sapper/app";
    import * as api from 'api.js';
    const { session, page } = stores();
    let connectionAs;
    let timeCommitment;
    let personalNoteRequest = "";
    let skillFluency = 0;
    let userData;
    console.log($page.query);
    const connectionOptions = [
        { id : 1, text: 'As a student'},
        { id : 2, text: 'As a co-learner'}
    ];
    async function submit(event) {

    }
    async function getData() {
		console.log($session.user.access_token);
		const data = await api.get(`users/${$page.query}`, $session.user.access_token);
		console.log(data);
		userData = data;
	}
	getData();
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
                            <h4 class="media-heading request-text">Connection request to </h4>
                        </div>
                    </div>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                Learning domain:
                            </div>
                            <div class="col-sm-8">

                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                Connection:
                            </div>
                            <div class="col-sm-8">
                                <select class="form-control form-control-md" value={connectionAs}>
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
