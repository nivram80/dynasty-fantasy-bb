<script>
  import { createEventDispatcher } from 'svelte';
  import IconPlus from "./IconPlus.svelte";
  import IconEdit from "./IconEdit.svelte";

  export let listTitle = ''
  export let prospects = [];

  const dispatch = createEventDispatcher();
</script>

<header>
  <h3>{listTitle}</h3>
  <div class="action-icon" on:click={() => dispatch('showAddPlayer')}>
    <IconPlus />
  </div>
</header>

<table class="player-list">
  <thead>
    <tr>
      <th class="small-cell sort" on:click={() => dispatch('sortByAlpha', 'position')}>Pos</th>
      <th class="small-cell sort" on:click={() => dispatch('sortByAlpha', 'team')}>Team</th>
      <th class="spacer" />
      <th class="text-left medium-cell sort" on:click={() => dispatch('sortByAlpha', 'lname')}>Name</th>
      <th class="small-cell sort" 
        title="MLB Pipeline"
        on:click={() => dispatch('sortByRankings', 'mlb')}>
        MLB
      </th>
      <th
        class="small-cell sort"
        title="Baseball America"
        on:click={() => dispatch('sortByRankings', 'baseballAmerica')}>
        BA
      </th>
      <th
        class="small-cell sort"
        title="Baseball Prospectus"
        on:click={() => dispatch('sortByRankings', 'baseballProspectus')}>
        BP
      </th>
      <th
        class="small-cell sort"
        title="Fan Graphs"
        on:click={() => dispatch('sortByRankings', 'fanGraphs')}>
        FG
      </th>
      <!-- <th class="small-cell">AVE</th> -->
    </tr>
  </thead>
  <tbody>
    {#each prospects as p}
      <tr class="player" class:bold={p.watchlist}>
        <td class="small-cell">{p.position.join()}</td>
        <td class="small-cell">{p.team}</td>
        <td class="spacer" />
        <td class="text-left medium-cell">
          {p.lname}, {p.fname}
        </td>
        <td class="small-cell blue" alt="MLB Pipeline Top 100">
          {p.rankings.mlb === 0 ? '--' : p.rankings.mlb}
        </td>
        <td class="small-cell red">
          {p.rankings.baseballAmerica === 0 ? '--' : p.rankings.baseballAmerica}
        </td>
        <td class="small-cell orange">
          {p.rankings.baseballProspectus === 0 ? '--' : p.rankings.baseballProspectus}
        </td>
        <td class="small-cell green">
          {p.rankings.fanGraphs === 0 ? '--' : p.rankings.fanGraphs}
        </td>
        <!-- <td class="small-cell">{rankingAve(p.rankings)}</td> -->
        <td class="small-cell action-icon" on:click={dispatch('displayEditForm', p)}>
          <IconEdit />
        </td>
      </tr>
    {/each}
  </tbody>
</table>