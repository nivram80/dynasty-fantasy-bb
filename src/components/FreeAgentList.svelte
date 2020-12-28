<script>
  import IconEdit from "./IconEdit.svelte";
  import DividerLabel from "./DividerLabel.svelte";
  import PlayerEdit from "./PlayerEdit.svelte";

  export let label = '';
  export let players = [];

  let selectedPlayer = null;
  let showEditPlayer = false;

  const displayEditForm = player => {
    selectedPlayer = player;
    showEditPlayer = true;
  };
</script>

<DividerLabel label={label} />
<table class="player-list">
  <thead>
    <tr>
      <th class="small-cell">Pos</th>
      <th class="small-cell">Team</th>
      <th class="spacer" />
      <th class="text-left medium-cell">Name</th>
    </tr>
  </thead>
  <tbody>
    {#each players as player}
      <tr class="player" class:bold={player.watchlist}>
        <td class="small-cell">{player.position.join()}</td>
        <td class="small-cell">{player.team}</td>
        <td class="spacer" />
        <td class="text-left medium-cell">{player.lname}, {player.fname}</td>
        <td class="small-cell action-icon" on:click={displayEditForm(player)}>
          <IconEdit />
        </td>
      </tr>
    {/each}
  </tbody>
</table>

{#if showEditPlayer}
  <PlayerEdit
    player={selectedPlayer}
    on:cancel={() => (showEditPlayer = false)} />
{/if}

<style>
  .player-list {
    margin-bottom: 20px;
  }
</style>