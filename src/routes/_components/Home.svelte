<script>
	import { goto, stores } from "@sapper/app";
	import * as api from 'api.js';
	import SearchList from './SearchList.svelte';
	
	let searchText = '';
	let selection = [];
	const { session } = stores();
	let peopleList = [];

	function onClose(index) {
		peopleList.splice(index, 1);
		peopleList = peopleList;
	}
	async function getData() {
		console.log($session.user.access_token);
		const userList = await api.get('users', $session.user.access_token);
		console.log(userList);
		peopleList = userList;
	}
	getData();

</script>

<style>
.full-width{
	width: 100%;
}
.divider-grey{
	width: 100%;
	padding: 0.2rem;
    border-bottom: solid 4px #d3d3d3;
}
.suggestions-text{
	color: #7ed5b7;
}
.help-text{
	font-size: 1.2rem
}
.no-margin{
	margin: 0;
}
.help-text-container{
	padding-top: 10px;
}
.send-request-container{
	text-align: center;
}
.send-requests{
	width: 50%;
}
</style>

<svelte:head>
	<title>Learning Web</title>
</svelte:head>

<div class="home-page">
	<div class="container page">
		<div class="row">
			<div class="col-md-8 offset-md-2 col-xs-12">
				<div class="input-group">
					<div class="input-group-btn">
					<button class="btn btn-default" type="submit">
						<ion-icon name="search-outline"></ion-icon>
					</button>
					</div>
					<input type="text" class="form-control" placeholder="Search for the topic you wish to learn" bind:value={searchText}>
				</div>
				<hr />
				<div class="row">
					<div class="col-md-6">
						<button type="button" class="btn full-width">View recieved requests</button>
					</div>
					<div class="col-md-6">
						<button type="button" class="btn full-width">View sent requests</button>
					</div>
				</div>
				<div class="divider-grey"></div>
				<p class="no-margin help-text-container">
					<ion-icon name="globe-outline"></ion-icon>
					<span class="help-text">People you may find helpful.</span>
				</p>
				<p class="no-margin suggestions-text">Suggestions are based upon learning interests.</p>
				<div class="divider-grey"></div>
				<div class="search-list-container">
					{#each peopleList as list, i}
						<SearchList listData={list} onDismiss={onClose} index={i} selection={selection} />
					{/each}
				</div>
				<div class="send-request-container">
					<button type="button" class="btn btn-primary send-requests">Send Requests</button>
				</div>
			</div>
		</div>
	</div>
</div>