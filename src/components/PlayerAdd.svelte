<script>
  import Dialog from './Dialog.svelte';
  import Card from './Card.svelte';
  import PlayerEditAddForm from './PlayerEditAddForm.svelte';

  export let prospect = false;
</script>

<Dialog>
  <Card>
    <h3>Add Player</h3>
    <PlayerEditAddForm {prospect} on:cancel />
  </Card>
</Dialog>