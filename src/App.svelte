<script>
	import Prospects from './components/Prospects.svelte';
</script>

<main>
	<Prospects />
</main>

<style>
	main {
		color: #333;
		font-family: system-ui, "Helvetica Neue", Helvetica, Arial, sans-serif;
	}

	:global(h1, h2, h3) {
		margin: 0 0 5px 0;
		padding: 0;
	}

	:global(.center) {
		text-align: center;
	}

	:global(.label) {
		display: block;
		margin-bottom: 2px;
		font-size: 10px;
	}

	:global(select, textarea) {
		font-family: inherit;
		font-size: inherit;
		box-sizing: border-box;
		border: 1px solid #ccc;
		border-radius: 2px;
		background-color: rgb(170, 62, 62);
		color: #fff;
	}

	:global(.form-row) {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 8px;
	}

	:global(.action-buttons) {
		margin-top: 20px;
	}
</style>