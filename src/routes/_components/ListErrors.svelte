<script>
	export let errors;
	console.log("List errors:" + errors);
</script>

{#if errors}
	<ul class="error-messages">
		<!--{#each Object.keys(errors) as key}-->
			<!--<li>{key} {errors[key]}</li>-->
			<li>{errors}</li>
		<!--{/each}-->
	</ul>
{/if}