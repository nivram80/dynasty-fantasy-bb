<script>
  import { onMount, onDestroy } from "svelte";
  import IconPlus from "./IconPlus.svelte";
  import PlayerList from "./PlayerList.svelte";
  import PlayerAdd from "./PlayerAdd.svelte";

  let players = {
    startingPitchers: [],
    reliefPitchers: [],
    droppingPlayers: [],
  };
  let showAddPlayer = false;
  let unsubscribeStartingPitchers;
  let unsubscribeReliefPitchers;
  let unsubscribeDroppingPlayers;

  onMount(() => {
    getStartingPitchers();
    getReliefPitchers();
    getDroppingPlayers();
  });

  onDestroy(() => {
    unsubscribeStartingPitchers();
    unsubscribeReliefPitchers();
    unsubscribeDroppingPlayers();
  });

  const getStartingPitchers = () => {
    unsubscribeStartingPitchers = db.collection("players")
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
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
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains', 'RP')
      .onSnapshot(snapshot => {
        players['reliefPitchers'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getDroppingPlayers = () => {
    unsubscribeReliefPitchers = db.collection("players")
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', true)
      .onSnapshot(snapshot => {
        players['droppingPlayers'] = snapshot.docs.map(doc => {
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

  <header>
    <h3>Dropping</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <PlayerList players={players.droppingPlayers} label="NEED TO DROP 7" />
</section>

{#if showAddPlayer}
  <PlayerAdd on:cancel={() => (showAddPlayer = false)} />
{/if}