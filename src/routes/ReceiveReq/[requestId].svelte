<script context="module">

import * as api from 'api.js';


export async function preload({ params }) {
		console.log("pre-load");
		const {requestId} = params;
		console.log(params);
		return {requestId};
	}

</script>

<script>

import { onMount } from 'svelte';
import { stores } from '@sapper/app';
import ListErrors from '../_components/ListErrors.svelte';
import RequestForm from './RequestForm.svelte';
	
export let requestId;
let learningConnectionRequest;
let menteeDetails;
let errors;


console.log("requestid" + requestId);

const { session } = stores();

console.log("On Page Load");
console.log(requestId);
onMount(() => {    
		api.get(`learning_connections/${requestId}`,$session.user.access_token).then((resLearningConnection) => {
        console.log(resLearningConnection);
		api.get(`users/${resLearningConnection.mentee.id}`,$session.user.access_token).then((resUser)=>{
			console.log(resUser);
			menteeDetails = resUser;
			learningConnectionRequest = resLearningConnection;
		});
	});
});
	
async function save(event) {
	/*	inProgress = true;

		const response = await post(`auth/save`, event.detail);

		errors = response.errors;
		if (response.user) $session.user = response.user;

		inProgress = false;*/
	}

</script>

<!--<script>
	import { goto, stores } from '@sapper/app';
	import ListErrors from '../_components/ListErrors.svelte';
	import RequestForm from './RequestForm.svelte';
	import { post } from 'utils.js';

	export let requestId;
	console.log("Request Id for passing to Request form is" + requestId);
	let inProgress;
	let errors;

	const { session } = stores();

	async function logout() {
		await post(`auth/logout`);
		$session.user = null;
		goto('/');
	}

	async function save(event) {
		inProgress = true;

		const response = await post(`auth/save`, event.detail);

		errors = response.errors;
		if (response.user) $session.user = response.user;

		inProgress = false;
	}
</script>-->

<svelte:head>
	<title>Request</title>
</svelte:head>

<div class="settings-page">
	<div class="container page">
		<div class="row">
			<div class="col-md-6 offset-md-3 col-xs-12">

				<h1 class="text-xs-center">Request from..</h1>

				<ListErrors {errors}/>

				<!--<RequestForm requestId={requestId} on:save={save} {...$session.user} {inProgress}/>-->
				<RequestForm learningConnectionRequest={learningConnectionRequest} menteeDetails={menteeDetails} on:save={save}/>
				
			</div>
		</div>
	</div>
</div>