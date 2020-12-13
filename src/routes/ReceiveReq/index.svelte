<script>
import { goto, stores } from "@sapper/app";
import * as api from 'api.js';
import ReceiveReqList from './ReceiveReqList.svelte';
import { onMount } from 'svelte';
import ButtonBar from '../_components/ButtonBar.svelte'

const { session } = stores();
let displayMode = "";
let peopleList;

onMount(() =>  {	
		console.log("getrecvdrequests called");
		if($session.user.access_token === undefined)
		{
			goto('/login');
		}
		console.log($session.user.access_token);
		api.get(`learning_connections?status=pending`, $session.user.access_token).then((connectionList)=>
            {
            console.log(connectionList);
            const listData = connectionList.filter(conn => conn.mentor.id === $session.user.userid);
            console.log(listData);
            peopleList = listData;
            displayMode = "ViewReceivedRequestList"
        });
	});
</script>

<style>

.divider-grey{
	width: 100%;
	padding: 0.2rem;
    border-bottom: solid 4px #d3d3d3;
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
.request-container{
	text-align: ceter;
}
</style>

<svelte:head>
	<title>Learning Web</title>
</svelte:head>

<div class="home-page">
	<div class="container page">
        <div class="divider-grey"></div>
        <div class="row">
			<div class="col-md-8 offset-md-2 col-xs-12">
            <ButtonBar/>
                <p class="no-margin help-text-container">
                    <ion-icon name="globe-outline"></ion-icon>
                    <span class="help-text">Received connection requests</span>
                </p>
                <div class="divider-grey"></div>
                <hr />
                {#if displayMode==="ViewReceivedRequestList"}
                <div class="request-container">
                    {#each peopleList as list, i}
                        <ReceiveReqList listData={list} displayMode={displayMode} />
                    {/each}
                </div>
                {/if}
            </div>
	</div>
    </div>
</div>


