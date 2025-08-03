<script lang="ts">
	let { children } = $props();
	let a = ["blog", "about", "game", "tool"];
</script>

<nav>
	<a href="/">home</a>
	{#each a as aa}
		<a href="/{aa}">{aa}</a>
	{/each}
</nav>

{@render children()}
