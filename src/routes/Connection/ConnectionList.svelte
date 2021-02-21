<script>
    export let listData;
    export let userId;
    export let onDismiss;
    export let index;
    export let selection;
    let relation;
    let name;
    
    if(userId === listData.mentee.id)
    {
        relation = "Mentor";
        name = listData.mentor.name;
    }
    else if(userId === listData.mentor.id)
    {
        relation = "Mentee";
        name = listData.mentee.name;
    }
</script>

<style>

.connection-list{
	position: relative;
	padding-top: 15px;
}
.image-container{
	text-align: center;
}
.connection-type{
	padding-top: 3px;
	font-weight: 600;
}
.relation-span{
    color: #8d8d8d;
}
.connection-type-span{
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
.aoe-span{  
	color: orange;
    font-weight: 600;
}
</style>

<div class="connection-list row">
    <div class="image-container col-sm-3">
        <img src="images/temp.jpg" class="img-circle" alt="Profile" width="80" height="80">
        <div class="connection-type">
            <span class="relation-span"> {(relation)} <span class="check-icon"><ion-icon name="checkmark-done-outline"></ion-icon></span></span>
            <span class="connection-type-span"> </span>
        </div>
    </div>
    <div class="desc-container col-sm-9">
        <a href="/Connection/{listData.id}">
            <div class="name-container">
                <span class="name">{name}</span>
            </div>
        </a>    
        <div class="aoe-container row">
            <div class="aoe-span col-xs-1">AOE:</div>
            <div class="col-xs-10">
                    <div>{listData.notes}</div>
            </div>
        </div>
    </div>
    <div class="col-xs-12">
        <hr />
    </div>
</div>