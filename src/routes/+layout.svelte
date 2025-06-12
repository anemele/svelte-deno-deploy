<script lang="ts">
	let { children } = $props();
</script>

<nav>
	<a href="/">home</a>
	<a href="/blog">blog</a>
	<a href="/about">about</a>
	<a href="/game">game</a>
</nav>

{@render children()}
