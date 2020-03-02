<script>
  import Dialog from './Dialog.svelte';
  import Card from './Card.svelte';
  import PlayerEditAddForm from './PlayerEditAddForm.svelte';

  export let player;
</script>

<Dialog>
  <Card>
    <h3>Edit Player</h3>
    <PlayerEditAddForm {player} isEdit={true} on:cancel />
  </Card>
</Dialog>