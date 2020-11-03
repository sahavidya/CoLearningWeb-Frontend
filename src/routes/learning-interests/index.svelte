<script>
    import { onMount } from 'svelte';
    import { goto, stores } from "@sapper/app";
    import ListErrors from '../_components/ListErrors.svelte';
    import * as api from 'api.js';

    const { session } = stores();

    let errors = null;
   
    errors = 'Messages are displayed here';

let userName; 
    let interests;
    let learningInterest1, learningInterest2; 
    let learningInterest3, learningInterest4;
    let skills;
    let expertiseDomain1, expertiseDomain2, expertiseDomain3, expertiseDomain4;
    let bio;
    let isLearner = true;
    let isGuide = true;
    
    function submit(event) {
   
       const response =  api.put("user", {
      username: userName,
         need_mentoring    : isLearner,
         available_to_mentors    : isGuide,
        skilss    : skills,
    interestss    : interests,
           bio: bio
    },
$session.user.access_token    
    );

   
    errors = 'After Put Call';

   
    }

    // Load information related to  skills and interests
    onMount(()=> {
        api.get(
		"user",
		$session.user.access_token
		).then((user)=> {
   userName= user.username;
       isLearner= user.need_mentoring;
    isGuide= user.available_to_mentor;
 skills= user.skills;
    interests= user.interests;
       bio= user.bio;
 
    // Todo - Split skills and interests and assignt to seperate variables
    learningInterest1= interests;
    learningInterest2= interests;
    learningInterest3= interests;
    learningInterest4= interests;

  expertiseDomain1 = skills;
   expertiseDomain2 = skills;
    expertiseDomain3 = skills;
        expertiseDomain4 = userName;
      
    
        })

	})


</script>

<style>
    /* The switch - the box around the slider */
    .switch {
        position: relative;
        display: inline-block;
        width: 60px;
        height: 34px;
    }

    /* Hide default HTML checkbox */
    .switch input {
        opacity: 0;
        width: 0;
        height: 0;
    }

    /* The slider */
    .slider {
        position: absolute;
        cursor: pointer;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: #ccc;
        -webkit-transition: .4s;
        transition: .4s;
    }

    .slider:before {
        position: absolute;
        content: "";
        height: 26px;
        width: 26px;
        left: 4px;
        bottom: 4px;
        background-color: white;
        -webkit-transition: .4s;
        transition: .4s;
    }

    input:checked + .slider {
        background-color: #2196F3;
    }

    input:focus + .slider {
        box-shadow: 0 0 1px #2196F3;
    }

    input:checked + .slider:before {
        -webkit-transform: translateX(26px);
        -ms-transform: translateX(26px);
        transform: translateX(26px);
    }

    /* Rounded sliders */
    .slider.round {
        border-radius: 34px;
    }

    .slider.round:before {
        border-radius: 50%;
    }

    /* Page styling */
    .divider-grey{
        padding-bottom: 1rem;
        border-top: solid 4px #d3d3d3;
    }
    .green-text{
        color: #7ed5b7;
    }
    .grey-text{
        color: cadetblue;
        font-weight: 700;
    }
    .orange-text{
        color: #ffaa3c;
    }
</style>

<svelte:head>
	<title>Skills Profile</title>
</svelte:head>

    <div class="container page">
		<div class="row">
<div class="update-user-page">
            <div class="col-md-8 offset-md-2 col-xs-12">
                <h1 class="text-xs-center">Learning and Sharing Profiles</h1>
                <hr />
                <p class="desc-text green-text">Please provide us inputs to serve you better. You can either be a learner or guide or both at the same time.</p>
                <div class="divider-grey"></div>


         <ListErrors {errors} />
               <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-10">Will you like to use this platform to educate yourself?</div>
                            <div class="col-sm-2">
                                <label class="switch">
                                    <input type="checkbox" bind:checked={isLearner}>
                                    <span class="slider round"></span>
                                </label>
                            </div>
                        </div>
                    </fieldset>
                    <p class="desc-text grey-text">Let us know your learning interests:</p>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isLearner} type="text" bind:value={learningInterest1}></div>
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isLearner} type="text" bind:value={learningInterest2}></div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isLearner} type="text" bind:value={learningInterest3}></div>
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isLearner} type="text" bind:value={learningInterest4}></div>
                        </div>
                    </fieldset>
                    <div class="divider-grey"></div>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-10">Will you like to be a guide for other learners?</div>
                            <div class="col-sm-2">
                                <label class="switch">
                                    <input type="checkbox" bind:checked={isGuide}>
                                    <span class="slider round"></span>
                                </label>
                            </div>
                        </div>
                    </fieldset>
                    <p class="desc-text green-text">Let us know the domains of your expertise:</p>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isGuide} type="text" bind:value={expertiseDomain1}></div>
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isGuide} type="text" bind:value={expertiseDomain2}></div>
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isGuide} type="text" bind:value={expertiseDomain3}></div>
                            <div class="col-sm-6"><input class="form-control form-control-md input-desc" disabled={!isGuide} type="text" bind:value={expertiseDomain4}></div>
                        </div>
                    </fieldset>
                    <p class="desc-text orange-text">Please write down somethine interesting that how your guidance coule be helpful for learners:</p>
                    <fieldset class="form-group">
                        <textarea class="form-control" disabled={!isGuide} rows="5" bind:value={bio}></textarea>
                    </fieldset>
                    <fieldset class="form-group">
                        <button class="btn btn-md btn-primary pull-xs-center">
                            Update
                        </button>
                    </fieldset>
                    <div class="divider-grey"></div>
                </form>
            </div>
        </div>
    </div>
</div>