<script>
    import { onMount } from 'svelte';
    export let listData;
    export let onDismiss;
    export let index;
    export let selection;
    let skillsArr = [""];

     onMount(()=> {
         if(listData.skills!=undefined)
         {
            skillsArr = listData.skills.split("|");
         }
     })
</script>

<style>
.remove{
	position: absolute;
	right: 20px;
    top: 10px;
}
.search-list{
	position: relative;
	padding-top: 15px;
}
.image-container{
	text-align: center;
}
.user-type{
	padding-top: 3px;
	font-weight: 600;
}
.guide-span{
    color: #8d8d8d;
}
.co-learner-span{
    color: #7ed5b7;
}
.name-container{
	width: 100%;
}
.name{
	font-size: 22px;
    color: gray;
    font-weight: 600;
}
.is-new-suggestion{
	color: deepskyblue;
	margin-left: 10px;
}
.aoe-span{
	color: orange;
    font-weight: 600;
}
.no-display{
    display: none;
}
.selection-checkbox{
    position: relative;
    top: 40%;
}
.details-container{
    text-align: right;
    width: 90%;
}
</style>

<div class="search-list row">
    <div class="col-sm-1">
        <!-- <input type="checkbox" class="form-check-input selection-checkbox" bind:group={selection} value={index} /> -->
    </div>
    <div class="image-container col-sm-2">
        <img src="images/temp.jpg" class="img-circle" alt="Profile" width="80" height="80">
        <div class="user-type">
            <span class={`guide-span ${(listData.available_to_mentor) ? '' : 'no-display'}`}>Guide <span class="check-icon"><ion-icon name="checkmark-done-outline"></ion-icon></span></span>
            <span class={`co-learner-span ${(listData.is_available) ? '' : 'no-display'}`}>Co-Learner</span>
        </div>
    </div>
    <div class="desc-container col-sm-9">
        <div class="name-container">
            <span class="name">{listData.name}</span>
            <!--<span class={`is-new-suggestion ${(listData.isNewSuggestion) ? '' : 'no-display'}`}>New Suggestion</span>-->
        </div>
        <div class="aoe-container row">
            <div class="aoe-span col-xs-1">AOE:</div>
            <div class="col-xs-10">
                {#each skillsArr as expertise}
                    <div>{expertise}</div>
                {/each}
            </div>
        </div>
       <div class="details-container">
            <a href="/mentor-profile?mentorId={listData.id}">
                <button type="button" class="btn btn-info btn-sm">View Profile</button>
            </a>
        </div>
    </div>
    <div class="remove">
        <button type="button" class="close" aria-label="Close" on:click={() => onDismiss(index)}>
            <span aria-hidden="true">&times;</span>
        </button>
    </div>
    <div class="col-xs-12">
        <hr />
    </div>
</div>