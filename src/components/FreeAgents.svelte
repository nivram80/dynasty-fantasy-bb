<script>
  import { onMount, onDestroy } from "svelte";
  import PlayerAdd from "./PlayerAdd.svelte";
  import PlayerEdit from "./PlayerEdit.svelte";
  import IconPlus from "./IconPlus.svelte";
  import IconEdit from "./IconEdit.svelte";
  import DividerLabel from "./DividerLabel.svelte";

  let players = [];
  let selectedPlayer = null;
  let showAddPlayer = false;
  let showEditPlayer = false;
  let unsubscribe;

  onMount(() => getPlayers());

  onDestroy(() => unsubscribe());

  const displayEditForm = player => {
    selectedPlayer = player;
    showEditPlayer = true;
  };

  const getPlayers = () => {
    unsubscribe = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .onSnapshot(snapshot => {
        players = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };
</script>

<section class="page-column small">
  <header>
    <h3>Free Agent Watch</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <DividerLabel label="OUTFIELDERS" />
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
          <td class="small-cell">{player.position}</td>
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
</section>

{#if showAddPlayer}
  <PlayerAdd on:cancel={() => (showAddPlayer = false)} />
{/if}

{#if showEditPlayer}
  <PlayerEdit
    player={selectedPlayer}
    on:cancel={() => (showEditPlayer = false)} />
{/if}
