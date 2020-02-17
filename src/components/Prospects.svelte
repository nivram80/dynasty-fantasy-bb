<script>
  import { onMount } from "svelte";
  // import firebase from 'firebase/app'
  // import { prospects } from "../data/prospects";

  let players = [];

  onMount(() => {
    db.collection('players').get()
      .then((snapshot) => {
        players = snapshot.docs.map(doc => doc.data());
      });
  });

  const rankingAve = rankings => {
    let rnks = [];
    if (rankings.mlb !== 9999) rnks.push(rankings.mlb);
    if (rankings.baseballAmerica !== 9999) rnks.push(rankings.baseballAmerica);
    if (rankings.baseballProspectus !== 9999) rnks.push(rankings.baseballProspectus);
    return Math.floor((rnks.reduce((a, b) => a + b, 0)) / rnks.length);
  }

  const sortPlayers = column => {
    players = players.sort((a, b) => a.rankings[column] - b.rankings[column]);
  }
</script>

<h3>Top 100 Available Prospects</h3>

<table class="player-list">
  <thead>
    <tr>
      <th class="small-cell">Pos</th>
      <th class="small-cell">Team</th>
      <th class="spacer"></th>
      <th class="text-left medium-cell">Name</th>
      <th class="small-cell sort" on:click={() => sortPlayers('mlb')}>MLB</th>
      <th class="small-cell sort" on:click={() => sortPlayers('baseballAmerica')}>BA</th>
      <th class="small-cell sort" on:click={() => sortPlayers('baseballProspectus')}>BP</th>
      <th class="small-cell">AVE</th>
    </tr>
  </thead>
  <tbody>
    {#each players as player}
      <tr class="player" class:bold={player.watchlist}>
        <td class="small-cell">{player.position}</td>
        <td class="small-cell">{player.team}</td>
        <th class="spacer"></th>
        <td class="text-left medium-cell">{player.lname}, {player.fname}</td>
        <td class="small-cell blue">{player.rankings.mlb === 9999 ? '--' : player.rankings.mlb}</td>
        <td class="small-cell red">{player.rankings.baseballAmerica === 9999 ? '--' : player.rankings.baseballAmerica}</td>
        <td class="small-cell green">{player.rankings.baseballProspectus === 9999 ? '--' : player.rankings.baseballProspectus}</td>
        <th class="small-cell">{rankingAve(player.rankings)}</th>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  h3 {
    margin: 0;
    padding: 0;
  }

  table {
    border-spacing: 0;
    font-size: 10px;
  }

  thead > tr {
    background: #ddd;
  }


  thead th {
    padding: 5px 0;
    font-size: 10px;
  }

  tr:nth-child(even) {
    background-color: #f4f4f4;
  }
  
  tr:hover {
    background: #eee;
  }

  td {
    padding: 4px 0;
    text-align: center;
  }

  .bold {
    font-weight: bold;
  }

  .medium-cell {
    width: 125px;
  }

  .small-cell {
    width: 35px;
  }

  .spacer {
    width: 15px;
  }

  .text-left {
    text-align: left;
  }

  .blue {
    background: rgba(0, 0, 255, 0.15);
  }

  .red {
    background: rgba(255, 0, 0, 0.15);
  }

  .green {
    background: rgba(0, 128, 0, 0.15);
  }

  .sort {
    cursor: pointer;
  }

  .sort:hover {
    text-decoration: underline;
  }
</style>