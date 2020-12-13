<script>
    
export let listData;
export let displayMode;

//date conversion
let creationDate = new Date (Date.parse(listData.creation_date));
console.log(creationDate);
let creationDateFormatted = creationDate.toDateString();

</script>

<style>

.connection-list{
	position: relative;
	padding-top: 15px;
}
.name-container{
	width: 100%;
}
.aoe-span{
	color: orange;1
    font-weight: 600;
}

.name{
	font-size: 22px;
    color: gray;
    font-weight: 600;
}

</style>

<!--<div class="connection-list row">
    <div class="desc-container col-sm-12 col-md-9 col-lg-9 col-xl-9">-->
        {#if displayMode==="ViewReceivedRequestList"}
        <div class="name-container">
            <span class="name">{listData.mentee.name}</span>
        </div>
        {/if}
        <div class="container row">
            <div class="aoe-span col-sm-4">Skill to learn:</div>
            <div class="name-container col-sm-8">
                    {(listData.skillname)}
            </div>
        </div>
        <div class="container-row">
            <div class="aoe-span col-sm-4">Request date:</div>
            <div class="name-container col-sm-8">
                    {(creationDateFormatted)}
            </div>
        </div>
        <div class="container-row col-sm-12">
            <a href="/ReceiveReq/{listData.id}">
                <button type="button" class="btn btn-info btn-sm">View</button>
            </a>
        </div>
    <!--</div>
</div>-->