<script>
  import { onMount, createEventDispatcher } from "svelte";

  export let autofocus = false;
  export let fieldName;
  export let label = "";
  export let size = "medium"; // 'small', 'medium', or 'large'
  export let type = "text";
  export let value = "";

  const dispatch = createEventDispatcher();

  let inputEl;

  onMount(() => {
    if (autofocus) inputEl.focus();
  });

  const onInput = e => {
    // https://stackoverflow.com/questions/57392773/error-type-attribute-cannot-be-dynamic-if-input-uses-two-way-binding/57393751#57393751
    value = type.match(/^(number|range)$/) ? +e.target.value : e.target.value;
    dispatch("input", value);
  };
</script>

<div class="input-container">
  <label class="label" for={fieldName}>{label}</label>
  <input
    class={`input ${size}`}
    {type}
    id={fieldName}
    bind:this={inputEl}
    {value}
    autocomplete="off"
    on:input={onInput} />
</div>

<style>
  .input {
    height: 28px;
    padding: 0 0 0 8px;
    border: 1px solid #ccc;
    border-radius: 2px;
    box-sizing: border-box;
    background-color: #f6f6f6;
    font-size: 12px;
  }

  .small {
    width: 50px;
  }

  .medium {
    width: 100px;
  }

  .large {
    width: 150px;
  }
</style>