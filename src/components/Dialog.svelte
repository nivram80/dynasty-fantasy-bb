<div class="dialog">
  <slot />
</div>

<style>
  .dialog {
    position: fixed;
    top: 0; right: 0; bottom: 0; left: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.25);
    z-index: 2;
  }
</style>