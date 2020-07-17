<script>
    export let listData;
    export let onDismiss;
    export let index;
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
</style>

<div class="search-list row">
    <div class="image-container col-sm-2">
        <img src={listData.profileImage} class="img-circle" alt="Profile" width="80" height="80">
        <div class="user-type">
            <span class={`guide-span ${(listData.isGuide) ? '' : 'no-display'}`}>Guide <span class="check-icon"><ion-icon name="checkmark-done-outline"></ion-icon></span></span>
            <span class={`co-learner-span ${(listData.isCoLearner) ? '' : 'no-display'}`}>Co-Learner</span>
        </div>
    </div>
    <div class="desc-container col-sm-10">
        <div class="name-container">
            <span class="name">{listData.name}</span>
            <span class={`is-new-suggestion ${(listData.isNewSuggestion) ? '' : 'no-display'}`}>New Suggestion</span>
        </div>
        <div class="aoe-container row">
            <div class="aoe-span col-xs-1">AOE:</div>
            <div class="col-xs-10">
                {#each listData.aoe as extertise, i}
                    <div>{(i+1)}. {extertise}</div>
                {/each}
            </div>
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