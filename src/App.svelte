<script>
	import Prospects from './components/Prospects.svelte';
	import FreeAgents from './components/FreeAgents.svelte';
</script>

<main>
	<Prospects />
	<FreeAgents />
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}

	main {
		color: #333;
		font-family: system-ui, "Helvetica Neue", Helvetica, Arial, sans-serif;
		display: flex;
		justify-content: flex-start;
		align-items: flex-start;
	}

	:global(h1, h2, h3) {
		margin: 0 0 5px 0;
		padding: 0;
	}

	:global(.page-column) {
		margin-top: 10px;
		margin-left: 10px;
	}

	:global(.page-column.large) {
		width: 350px;
	}

	:global(.page-column.small) {
		width: 245px;
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

	:global(header) {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: calc(100% - 36px); 
  }

  :global(.player) {
    height: 22px;
  }

  :global(.player .action-icon) {
    display: none;
  }

  :global(.player:hover .action-icon, .action-icon:hover) {
    display: block;
  }

  :global(.action-icon) {
    background-color: #fff;
    cursor: pointer;
  }

  :global(table) {
    border-spacing: 0;
    font-size: 10px;
  }

  :global(thead > tr) {
    background: #ddd;
  }

  :global(thead th) {
    padding: 5px 0;
    font-size: 10px;
  }

  :global(tr:nth-child(even)) {
    background-color: #f4f4f4;
  }

  :global(td) {
    height: 100%;
    padding: 0;
    text-align: center;
  }

  :global(.bold) {
    font-weight: bold;
  }

  :global(.medium-cell) {
    width: 125px;
  }

  :global(.small-cell) {
    width: 35px;
  }

  :global(.spacer) {
    width: 15px;
  }

  :global(.text-left) {
    text-align: left;
  }

  :global(.blue) {
    background: rgba(0, 0, 255, 0.15);
  }

  :global(.red) {
    background: rgba(255, 0, 0, 0.15);
  }

  :global(.green) {
    background: rgba(0, 128, 0, 0.15);
  }

  :global(.sort) {
    cursor: pointer;
  }

  :global(.sort:hover) {
    text-decoration: underline;
  }
</style>