<script>
  import Dialog from './Dialog.svelte';
  import Card from './Card.svelte';
  import PlayerEditAddForm from './PlayerEditAddForm.svelte';

  export let prospect = false;
  export let own = false;
  export let available = true;
</script>

<Dialog>
  <Card>
    <h3>Add Player</h3>
    <PlayerEditAddForm {prospect} {own} {available} on:cancel />
  </Card>
</Dialog>
