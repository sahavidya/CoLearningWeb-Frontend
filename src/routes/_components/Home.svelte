<script>
	import { goto, stores } from "@sapper/app";
	import * as api from 'api.js';
	import SearchList from './SearchList.svelte';
	import ButtonBar from './ButtonBar.svelte'
	
	let searchText = '';
	let selection = [];
	const { session } = stores();
	let peopleList = [];

	function onClose(index) {
		peopleList.splice(index, 1);
		peopleList = peopleList;
	}

	async function getData() {
		console.log("getData called");
		if($session.user.access_token === undefined)
		{
			goto('/login');
		}
		console.log($session.user.access_token);
		/*const userList = await api.get(
		"users?skill=" + encodeURIComponent(searchText),
		$session.user.access_token
		);*/
		const userList = await api.get(
		"users",
		$session.user.access_token
		);
		console.log(userList);
		peopleList = userList;
	}

		console.log("Calling get data");	
		getData();

	async function getSentRequests() {	
		console.log("getsendrequests called");
		if($session.user.access_token === undefined)
		{
			goto('/login');
		}
		console.log($session.user.access_token);
		console.log($session.user.userid);
		const connectionList = await api.get(`learning_connections?status=pending`, $session.user.access_token);
		console.log(connectionList);
		const listData = await connectionList.filter(conn => conn.mentee.id === $session.user.userid);
    	console.log(listData);
		peopleList = listData;
		displayMode = "ViewSentRequestList"
	}

	async function getReceivedRequests() {	
		console.log("getrecvdrequests called");
		if($session.user.access_token === undefined)
		{
			goto('/login');
		}
		else
		{
			goto('/ReceiveReq');
		}
		/*console.log($session.user.access_token);
		const connectionList = await api.get(`learning_connections?status=pending`, $session.user.access_token);
		console.log(connectionList);
		const listData = connectionList.filter(conn => conn.mentor.id === $session.user.userid);
    	console.log(listData);
		peopleList = listData;
		displayMode = "ViewReceivedRequestList"*/
	}

	async function getConnections() {	
		console.log("getrecvdrequests called");
		if($session.user.access_token === undefined)
		{
			goto('/login');
		}
		else
		{
			goto('/Connection');
		}
	}
</script>

<style>
  .divider-grey {
    width: 100%;
    padding: 0.2rem;
    border-bottom: solid 4px #d3d3d3;
  }
  .suggestions-text {
    color: #7ed5b7;
  }
  .help-text {
    font-size: 1.2rem;
  }
  .no-margin {
    margin: 0;
  }
  .help-text-container {
    padding-top: 10px;
  }
</style>

<svelte:head>
  <title>Learning Web</title>
</svelte:head>

<div class="home-page">
  <div class="container page">
  <ButtonBar/>
    <div class="row">
      <div class="col-md-8 offset-md-2 col-xs-12">
        <div class="input-group">
          <div class="input-group-btn">
            <button class="btn btn-default" type="button" on:click={getData}>
              <ion-icon name="search-outline" />
            </button>
          </div>
          <input
            type="text"
            class="form-control"
            placeholder="Search for the topic you wish to learn"
            bind:value={searchText} />
        </div>
        <hr />
<!--<div class="row">
          <div class="col-md-6">
            <button type="button" class="btn full-width">
              View recieved requests
            </button>
          </div>
          <div class="col-md-6">
            <button type="button" class="btn full-width">
              View sent requests
            </button>
          </div>
        </div>-->
        <div class="divider-grey" />
        <p class="no-margin help-text-container">
          <ion-icon name="globe-outline" />
          <span class="help-text">People you may find helpful.</span>
        </p>
        <p class="no-margin suggestions-text">
          Suggestions are based upon learning interests.
        </p>
        <div class="divider-grey" />
        <div class="search-list-container">
          {#each peopleList as list, i}
		  	{#if list.Id != $session.user.userid}
            <SearchList
              listData={list}
              onDismiss={onClose}
              index={i}
              {selection} />
			{/if}
          {/each}
        </div>
      </div>
    </div>
  </div>
</div>
