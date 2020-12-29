<script>
  import { onMount, onDestroy } from "svelte";
  import IconPlus from "./IconPlus.svelte";
  import PlayerList from "./PlayerList.svelte";
  import PlayerAdd from "./PlayerAdd.svelte";

  let players = {
    startingPitchers: [],
    reliefPitchers: [],
  };
  let showAddPlayer = false;
  let unsubscribeStartingPitchers;
  let unsubscribeReliefPitchers;

  onMount(() => {
    getStartingPitchers();
    getReliefPitchers();
  });

  onDestroy(() => {
    unsubscribeStartingPitchers();
    unsubscribeReliefPitchers();
  });

  const getStartingPitchers = () => {
    unsubscribeStartingPitchers = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', false)
      .where('own', '==', true)
      .where('position', 'array-contains-any', ['SP', 'RHP', 'LHP', 'P'])
      .onSnapshot(snapshot => {
        players['startingPitchers'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getReliefPitchers = () => {
    unsubscribeReliefPitchers = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', false)
      .where('own', '==', true)
      .where('position', 'array-contains', 'RP')
      .onSnapshot(snapshot => {
        players['reliefPitchers'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };
</script>

<section class="page-column small">
  <header>
    <h3>Keepers - Pitchers</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <PlayerList players={players.startingPitchers} label="STARTING PITCHERS" />
  <PlayerList players={players.reliefPitchers} label="RELIEF PITCHERS" />
</section>

{#if showAddPlayer}
  <PlayerAdd on:cancel={() => (showAddPlayer = false)} />
{/if}