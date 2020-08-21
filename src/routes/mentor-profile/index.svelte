<script>
    import { stores } from "@sapper/app";
    import * as api from 'api.js';
    const userData = {
        Id: 1,
        username: "pbgurav",
        email: "pbgurav@gmail.com",
        hash: "e4ea89dd5af54632617daf7a5ae217287cdf7d39eee5a1d24fd8456b4c637c4f75d132eac5a23d2a06a60a398c0434eaa8b789d89b3fdcebfbd01e3ae86cc369c12e7038d79d9de9b43f890332a9ec17ca83f023c4dbb7f90ad67a288a9cc76720ef8934c78d9b95e5b51fb94b176cb7edc57a451462afa6e5914e25e578191d43cbdffd728ddc6502c035fc4d915b3d2544956422647ea04f5b8bf522087ca2c5383b525ad88ebe3068d65633d65849fc582f426f80daad5b0809e48d0ef63aeb803307b25fa578c403f11188bc7458565dce8565b7891151c9a44ca38392978b673e17f3c0e1aa8400696c77237e0f3502c6e4ffe412d711d9fe351125b257337b06e0e2ba2daa8436a42b23d791919901004bfa560c48d44ed9acc77910036a1b24d34234858591d941a8c22d969466100a22ccfcfe393bec0022bd83cb80e34cd80543efee71bd43e06c9f85026b0d74ffdd8f8f7f5fd0cb92a801fa1b22d6bb7efc50a27cc9f48f70900bb10c085e3800e1c6feae8d222c3534cb1bb2900f11d85d66a5bfe8e14a88bc708784c0e7fc4a0af8926b242c355fe03f8e69594752e51e5e8e2a22538f32715dcc383e8e3deb65853ca2a39e4c6f21bbba624bd61c886c13e23dc4af51163386934f15d5fb0b1273c0d434ed178df4642479a64b896959ac3ead897bc4a551624ab19a28a66168dfc4b146eb6278800487534a",
        salt: "d14c117e89cdc968b198b7416c71428e",
        firstname: "Prasad",
        lastname: "Gurav",
        country: null,
        state: null,
        city: null,
        emailprivacy: true,
        phonenumber: null,
        phonenumberprivacy: true,
        whatsappnumber: null,
        whatsappnumberprivacy: true,
        connectionprivacy: true,
        birthdate: null,
        gender: 1,
        occupation: null,
        photo: null,
        islearner: true,
        isguide: true,
        bio: null,
        iscolearner: false,
        istermsandconditionschecked: true,
        isemailverified: false,
        createdAt: "2020-07-25T17:05:54.005Z",
        updatedAt: "2020-07-25T17:05:54.005Z",
        Skills: [
            {
                skillname: "Gardening",
                UserSkills: {
                    "skilltype": 1
                }
            }
        ]
    };
    const { session, page } = stores();
    const {
        firstname,
        lastname,
        iscolearner,
        isguide,
        city,
        emailprivacy,
        email,
        phonenumberprivacy,
        phonenumber,
        whatsappnumberprivacy,
        whatsappnumber,
        bio
    } = userData;
    const languages = "English, Hindi, Marathi";
    async function getData() {
        console.log($page.query);
    }
    getData();
</script>

<style>
.aoe-span{
	color: orange;
    font-weight: 600;
}
</style>

<svelte:head>
	<title>Mentor Profile</title>
</svelte:head>

<div class="request-connect-page">
    <div class="container page">
		<div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <div class="row">
                    <div class="col-sm-4">
                        {firstname} {lastname}
                        <p>
                            <span class={`guide-span ${(isguide) ? '' : 'no-display'}`}>Guide</span>
                            <span class={`co-learner-span ${(iscolearner) ? '' : 'no-display'}`}>Co-Learner</span>
                        </p>
                    </div>
                    <div class="col-sm-8">
                        <div class="aoe-container row">
                            <div class="aoe-span col-xs-1">AOE:</div>
                            <div class="col-xs-10">
                                {#each userData.Skills as extertise, i}
                                    <div>{(i+1)}. {extertise.skillname}</div>
                                {/each}
                            </div>
                        </div>
                        <div>
                            {city}
                        </div>
                        <div>
                            {languages}
                        </div>
                    </div>
                </div>
                <div class="row">
                    {#if phonenumberprivacy && whatsappnumberprivacy && emailprivacy}
                        <p>This is a Private Account</p>
                    {:else if !phonenumberprivacy}
                        <p>Phone No: {phonenumber}</p>
                    {:else if !whatsappnumberprivacy}
                        <p>Whatsapp Number: {whatsappnumber}</p>
                    {:else if !emailprivacy}
                        <p>Email: {email}</p>
                    {/if}
                </div>
                <div class="row">
                    <p>{firstname}'s Words:</p>
                    <p>{bio}</p>
                </div>
            </div>
        </div>
    </div>
</div>