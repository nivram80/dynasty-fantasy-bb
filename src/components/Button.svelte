<script>
  import { createEventDispatcher } from 'svelte';

  export let type = 'primary'; // 'primary', 'secondary'

  const dispatch = createEventDispatcher();

</script>

<button
  class={`btn ${type}`}
  on:click={() => dispatch('click')}>
  <slot />
</button>

<style>
  .btn {
    width: 100%;
    height: 30px;
    margin-bottom: 8px;
    padding: 4px 0;
    box-sizing: border-box;
		border-radius: 2px;
		font-size: 12px;
		cursor: pointer;
		transition: all 0.25s ease;
  }

  .primary {
		border: 1px solid rgba(53, 92, 56);
    background-color: rgba(53, 92, 56);
    color: #fff;
  }

  .primary:hover {
		border: 1px solid rgba(34, 59, 36);
    background-color: rgba(34, 59, 36);
  }

	.secondary {
		border: 1px solid #ccc;
		background-color: #fff;
		color: #333;
	}

	.secondary:hover {
		background-color: #eee;
  }

  .danger {
		border: 1px solid transparent;
    background-color: transparent;
    color: rgb(139, 0, 0);
  }

  .danger:hover {
    background-color: rgb(139, 0, 0, 0.2);
  }
  
  .btn:last-child {
		margin-bottom: 0;
	}
</style>
