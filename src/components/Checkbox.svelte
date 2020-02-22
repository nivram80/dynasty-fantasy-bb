<script>
  import { createEventDispatcher } from "svelte";

  export let checked = false;
  export let fieldName;
  export let label = '';

  const dispatch = createEventDispatcher();

  const onInput = event => {
    dispatch('input', event.target.checked);
  }
</script>

<div class="checkbox-container">
  <input
    class="checkbox"
    type="checkbox"
    id={fieldName}
    name={fieldName}
    {checked}
    on:input={onInput} />
  <label class="label" for={fieldName}>{label}</label>
</div>

<style>
	.checkbox-container {
		display: inline-block;
		width: 100px;
	}

	.label {
		display: inline-block;
	}
</style>