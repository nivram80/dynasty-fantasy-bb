<script>
  export let label = '';
</script>

<div class="divider-label">
  <div class="line" />
  <label class="label">{label}</label>
</div>

<style>
  .divider-label {
    height: 20px;
    margin: 4px 0;
    text-align: center;
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
</style>