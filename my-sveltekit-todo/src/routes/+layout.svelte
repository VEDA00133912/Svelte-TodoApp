<script>
</script>

<header>My SvelteKit Todo App</header>

<main>
	<slot />
</main>

<footer>&copy; 2025 ryo_001339</footer>

<style>
	header {
		background-color: #6596e3;
		padding: 1em;
		color: white;
		font-weight: bold;
	}
	footer {
		background-color: #9596cd;
		padding: 0.5em;
		text-align: center;
		color: white;
		font-size: 0.9em;
		position: fixed;
		width: 100%;
		bottom: 0;
	}
	main {
		padding: 2em;
		margin-bottom: 3em;
	}
</style>
