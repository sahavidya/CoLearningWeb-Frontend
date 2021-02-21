<script context="module">

    import * as api from 'api.js';


    export async function preload({ params }) {
            console.log("pre-load");
            const {connectionId} = params;
            console.log(params);
            return {connectionId};
        }

</script>

<script>
import { onMount } from 'svelte';
import { stores } from '@sapper/app';

export let connectionId;
console.log("connectionId" + connectionId);

let learningConnectionRequest;
let userData;

const { session } = stores();

onMount(() => {
    console.log("OnMount");
    console.log(connectionId);
    api.get(`mentorship_relations`,$session.user.access_token).then((res) => {
        console.log(res);
       //learningConnectionRequest = res.filter(con => con.id == connectionId);
       learningConnectionRequest = res[0]; //Harcoded TBD filter by connectionId
       console.log(learningConnectionRequest)

       //get userdetails

       //TBD Display the connection and user details in a formatted manner
       api.get("users/" + res[0].mentor.id,$session.user.access_token).then((resUserProfile)=>{
               userData = resUserProfile; 
               console.log("User name:" +  userData.username);
               console.log("name:" +  userData.name);
               console.log("bio:" +  userData.bio);
        });
    });
});

</script>

<div>
<h1>Connection Request</h1>


{#if learningConnectionRequest !== undefined && userData !== undefined} 
{learningConnectionRequest.mentee.name}
{learningConnectionRequest.notes}
{learningConnectionRequest.mentor.name}
{userData.username}
{userData.name}
{userData.bio}
{/if}
</div>