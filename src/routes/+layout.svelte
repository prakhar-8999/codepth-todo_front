<script>
	import './styles.css';
	import "../app.css";
	import { Toaster } from 'svelte-french-toast';
</script>

<div class="app">
	<main>
		<slot />
	</main>
	<Toaster />
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>
