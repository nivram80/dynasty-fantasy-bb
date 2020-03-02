<script>
  export let label = '';
  export let line = false;
</script>

<div class="divider-label" class:left={!line}>
  {#if line}
    <div class="line" />
  {/if}
  <label class="label">{label}</label>
</div>

<style>
  .divider-label {
    height: 20px;
    margin: 4px 0;
    text-align: center;
  }

  .left {
    text-align: left;
  }

  .divider-label .line {
    position: relative;
    top: 8px;
    width: 100%;
    height: 1px;
    background-color: #ccc;

  }

  .divider-label .label {
    position: relative;
    top: -4px;
    display: inline-block;
    padding: 0 10px;
    background-color: #fff;
  }

  .divider-label.left .label {
    padding-left: 0;
  }
</style>