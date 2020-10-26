<script>
	import { goto, stores } from '@sapper/app';

	const { page, session } = stores();

	async function getReceivedRequests(){
        goto('/ReceiveReq');
    }
    
    async function getSentRequests(){
        goto('/ReceiveReq');
    }
    
    async function getConnections(){
        goto('/Connection');
	}
</script>


<hr/>
<div class="row" align="center">

    <a class="page-link ng-binding" href="/ReceiveReq">Received requests</a>

    <a class="page-link ng-binding" href="/ReceiveReq">Sent requests</a>

    <a class="page-link ng-binding" href="/Connection">Connections</a>

</div>
<hr/>