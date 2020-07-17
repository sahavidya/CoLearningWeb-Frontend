<script>
    export let updateValue = "";
    export let iconClass = "";
    export let descText = "";
    export let inputType = "text";
</script>

<div class="row">
    <div class="col-sm-4">
        <p class="desc-parent">
            <ion-icon name={iconClass}></ion-icon>
            <span class="detail-desc">{descText}</span>
        </p>
    </div>
    <div class="col-sm-8">
        <input class="form-control form-control-md input-desc" type=inputType placeholder={descText} bind:value={updateValue}>
    </div>
</div>