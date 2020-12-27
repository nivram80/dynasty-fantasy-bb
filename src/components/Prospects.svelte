<script>
  import { onMount, onDestroy } from "svelte";
  import PlayerAdd from "./PlayerAdd.svelte";
  import PlayerEdit from "./PlayerEdit.svelte";
  import IconPlus from "./IconPlus.svelte";
  import IconEdit from "./IconEdit.svelte";

  let players = [];
  let unsubscribe;
  let selectedPlayer = null;
  let showAddPlayer = false;
  let showEditPlayer = false;

  onMount(() => getPlayers());

  onDestroy(() => unsubscribe());

  const displayEditForm = player => {
    selectedPlayer = player;
    showEditPlayer = true;
  };

  const getPlayers = () => {
    unsubscribe = db.collection("players")
      .where("prospect", '==', true)
      .where('available', '==', true)
      .onSnapshot(snapshot => {
        players = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        sortPlayers("baseballAmerica");
      });
  };

  // const rankingAve = rankings => {
  //   let rnks = [];
  //   if (rankings.mlb !== 0) rnks.push(rankings.mlb);
  //   if (rankings.baseballAmerica !== 0) rnks.push(rankings.baseballAmerica);
  //   if (rankings.baseballProspectus !== 0)
  //     rnks.push(rankings.baseballProspectus);
  //   return Math.floor(rnks.reduce((a, b) => a + b, 0) / rnks.length);
  // };

  const sortPlayers = column => {
    // Based on https://stackoverflow.com/a/29829370.
    players = players.sort((a, b) => {
      return (
        (a.rankings[column] === 0) - (b.rankings[column] === 0) ||
        +(a.rankings[column] > b.rankings[column]) ||
        -(a.rankings[column] < b.rankings[column])
      );
    });
  };
</script>

<section class="page-column large">
  <header>
    <h3>Top 100 Available Prospects</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <table class="player-list">
    <thead>
      <tr>
        <th class="small-cell">Pos</th>
        <th class="small-cell">Team</th>
        <th class="spacer" />
        <th class="text-left medium-cell">Name</th>
        <th class="small-cell sort" on:click={() => sortPlayers('mlb')}>MLB</th>
        <th
          class="small-cell sort"
          on:click={() => sortPlayers('baseballAmerica')}>
          BA
        </th>
        <th
          class="small-cell sort"
          on:click={() => sortPlayers('baseballProspectus')}>
          BP
        </th>
        <th
          class="small-cell sort"
          on:click={() => sortPlayers('fanGraphs')}>
          FG
        </th>
        <!-- <th class="small-cell">AVE</th> -->
      </tr>
    </thead>
    <tbody>
      {#each players as player}
        <tr class="player" class:bold={player.watchlist}>
          <td class="small-cell">{player.position}</td>
          <td class="small-cell">{player.team}</td>
          <td class="spacer" />
          <td class="text-left medium-cell">{player.lname}, {player.fname}</td>
          <td class="small-cell blue" alt="MLB Pipeline Top 100">
            {player.rankings.mlb === 0 ? '--' : player.rankings.mlb}
          </td>
          <td class="small-cell red">
            {player.rankings.baseballAmerica === 0 ? '--' : player.rankings.baseballAmerica}
          </td>
          <td class="small-cell orange">
            {player.rankings.baseballProspectus === 0 ? '--' : player.rankings.baseballProspectus}
          </td>
          <td class="small-cell green">
            {player.rankings.fanGraphs === 0 ? '--' : player.rankings.fanGraphs}
          </td>
          <!-- <td class="small-cell">{rankingAve(player.rankings)}</td> -->
          <td class="small-cell action-icon" on:click={displayEditForm(player)}>
            <IconEdit />
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</section>

{#if showAddPlayer}
  <PlayerAdd prospect on:cancel={() => (showAddPlayer = false)} />
{/if}

{#if showEditPlayer}
  <PlayerEdit
    player={selectedPlayer}
    on:cancel={() => (showEditPlayer = false)} />
{/if}
