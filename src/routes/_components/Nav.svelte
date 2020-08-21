<script>
	import { stores } from '@sapper/app';

	const { page, session } = stores();

	async function logout() {
		// await post(`auth/logout`);
		$session.user = null;
		goto('/');
	}
</script>

<nav class="navbar navbar-light">
	<div class="container">
		<a rel='prefetch' class="navbar-brand" href=".">Learning Web</a>
		<ul class="nav navbar-nav pull-xs-right">
			{#if $session.user}
				<li class="nav-item">
					<a rel='prefetch' class="nav-link" class:active="{$page.path === '/Home'}" href="/Home">Home</a>
				</li>
				<li class="nav-item">
					<a rel='prefetch' href="/update-user" class="nav-link" class:active="{$page.path === '/update-user'}">
						Update User
					</a>
				</li>
				<li class="nav-item">
					<a rel='prefetch' href="/learning-interests" class="nav-link" class:active="{$page.path === '/learning-interests'}">
						Learning Interests
					</a>
				</li>
				<li class="nav-item">
					<a rel='prefetch' href="/login" class="nav-link" on:click={logout}>
						Log Out
					</a>
				</li>
			{:else}
				<li class="nav-item">
					<a rel='prefetch' href="/login" class="nav-link" class:active="{$page.path === '/login'}">
						Sign in
					</a>
				</li>

				<li class="nav-item">
					<a rel='prefetch' href="/register" class="nav-link" class:active="{$page.path === '/register'}">
						Sign up
					</a>
				</li>
			{/if}
		</ul>
	</div>
</nav>