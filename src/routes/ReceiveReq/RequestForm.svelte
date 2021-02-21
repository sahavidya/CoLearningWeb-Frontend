<!--<script>
	import { createEventDispatcher } from 'svelte';

	 let acceptText, declineText, username='Aniket',coLearnerUser ='Nilesh';
	 let learningdomain = 'Csharp';
	 let connectionString = 'Co Learner';
	 let communicationLanguages = 'Marathi,Hindi';
	 let currentCity = 'Pune';
	 let fluencyrating = '7/10 (self rated)';
	 let dailyTimeSpendonLearningin = '90 min';
	 let coLearnersPreferredMode = 'chat';
	 
	const dispatch = createEventDispatcher();

	function submit(event) {
		dispatch('save', { image, username, bio, email, password }); 
	}
</script>-->

<script>
import { goto, stores } from '@sapper/app';
import * as api from 'api.js';

export let learningConnectionRequest;
export let menteeDetails;

let acceptText;
let declineText;

console.log("Mentee Details:" + menteeDetails);

const { session } = stores();

 async function submit(event) {

		 let notes = "";
		 let command = "";
		if(event.submitter.name === "accept")
		{
			notes = acceptText;
			command = "accept";
		}
		else if(event.submitter.name === "reject")
		{
			notes = declineText;
			command = "reject";
		}

		console.log("Notes:" + notes);
		console.log("Command:" + command);
		console.log("Learning Connection Id:" + learningConnectionRequest.id);
		
		const response = await api.put(`mentorship_relation/${learningConnectionRequest.id}/${command}`,{},
		$session.user.access_token );

	goto('/Connection');
	console.log('submit clicked');
}

</script>

<form on:submit|preventDefault='{submit}'>
	
	{#if learningConnectionRequest !== undefined}
	<fieldset>
	<p class="text-warning">{learningConnectionRequest.mentee.name} wants to learn {learningConnectionRequest.notes} from you:</p>
		<table class="table">		
		<tbody>
			<tr>
				<td>Learning Domain:</td>
				<td>{learningConnectionRequest.notes}</td>
			</tr>
			<tr>
				<td>Connection:</td>
				<td>{learningConnectionRequest.notes}</td>
			</tr>
			<tr>
				<td>Communication Languages:</td>
				<!--<td>{menteeDetails.languages}</td>--> <!--//TBD-->
				<!--<td>{communicationLanguages}</td>user model-->
			</tr>
			<tr>
				<td>Current City:</td>
				<td>{menteeDetails.location}</td>
				<!--<td>{currentCity}</td>user model-->
			</tr>
			<tr>
				<td>fluency in {learningConnectionRequest.notes}:</td>
				<td>{learningConnectionRequest.notes}</td>
			</tr>
			<tr>
				<td>Time {learningConnectionRequest.mentee.name} can spend to learn {learningConnectionRequest.notes}:</td>
				<td>{learningConnectionRequest.notes}</td> 
			</tr>
			<tr>
				<td>{learningConnectionRequest.mentee.name} preferred mode of communication:</td>
				<td></td>
				<!--<td>{coLearnersPreferredMode}</td> user model-->
			</tr>
			<tr>
				<td>Message from {learningConnectionRequest.mentee.name}:</td>
				<td>{learningConnectionRequest.notes}</td>
			</tr>
		</tbody>
  		</table>
		
		<fieldset class="form-group">
			<p class="text-warning">Message to {learningConnectionRequest.mentee.name}:</p>
			<textarea class="form-control form-control-lg" rows="4" placeholder="Add your message here" bind:value={acceptText}/>
			<p class="text-warning">{learningConnectionRequest.mentee.name} is open to share his/her ideas with his/her co-learners.</p>
		</fieldset>			
		<button class="btn btn-outline-primary" name="accept">
				Accept request
		</button>
		<p class="text-primary">Once you accept the request your email id will be shared with {learningConnectionRequest.mentee.name}.</p>
		<hr/>		
		<fieldset class="form-group">
		<p class="text-danger">Don't want to accept the request? Please write down the reason so {learningConnectionRequest.mentee.name} can improve his/her skills.</p>
			<textarea class="form-control form-control-lg" rows="2" placeholder= "reason to decline request?" bind:value={declineText}/>
		</fieldset>
		<button class="btn btn-outline-danger" name="reject">
				Decline request
		</button>
	</fieldset>
	{/if}
</form>