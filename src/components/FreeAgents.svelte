<script>
  import { onMount, onDestroy } from "svelte";
  import IconPlus from "./IconPlus.svelte";
  import FreeAgentList from "./FreeAgentList.svelte";
  import PlayerAdd from "./PlayerAdd.svelte";
  
  let players = {
    catchers: [],
    firstBasemen: [],
    secondBasemen: [],
    thirdBasemen: [],
    shortstops: [],
    outfielders: [],
    startingPitchers: [],
    reliefPitchers: [],
  };
  let showAddPlayer = false;
  let unsubscribeCatchers;
  let unsubscribeFirstBasemen;
  let unsubscribeSecondBasemen;
  let unsubscribeThirdBasemen;
  let unsubscribeShortstops;
  let unsubscribeOutfielders;
  let unsubscribeStartingPitchers;
  let unsubscribeReliefPitchers;

  onMount(() => {
    getCatchers();
    getFirstBasemen();
    getSecondBasemen();
    getThirdBasemen();
    getShortstops();
    getOutfielders();
    getStartingPitchers();
    getReliefPitchers();
  });

  onDestroy(() => {
    unsubscribeCatchers();
    unsubscribeFirstBasemen();
    unsubscribeSecondBasemen();
    unsubscribeThirdBasemen();
    unsubscribeShortstops();
    unsubscribeOutfielders();
    unsubscribeStartingPitchers();
    unsubscribeReliefPitchers();
  });

  const getCatchers = () => {
    unsubscribeCatchers = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .where('position', 'array-contains', 'C')
      .onSnapshot(snapshot => {
        players['catchers'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getFirstBasemen = () => {
    unsubscribeFirstBasemen = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .where('position', 'array-contains', '1B')
      .onSnapshot(snapshot => {
        players['firstBasemen'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getSecondBasemen = () => {
    unsubscribeSecondBasemen = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .where('position', 'array-contains', '2B')
      .onSnapshot(snapshot => {
        players['secondBasemen'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getThirdBasemen = () => {
    unsubscribeThirdBasemen = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .where('position', 'array-contains', '3B')
      .onSnapshot(snapshot => {
        players['thirdBasemen'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getShortstops = () => {
    unsubscribeShortstops = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .where('position', 'array-contains', 'SS')
      .onSnapshot(snapshot => {
        players['shortstops'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getOutfielders = () => {
    unsubscribeOutfielders = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
      .where('position', 'array-contains-any', ['OF', 'LF', 'CF', 'RF'])
      .onSnapshot(snapshot => {
        players['outfielders'] = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
      });
  };

  const getStartingPitchers = () => {
    unsubscribeStartingPitchers = db.collection("players")
      .where('prospect', '==', false)
      .where('available', '==', true)
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
      .where('available', '==', true)
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
    <h3>Free Agent Watch</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <FreeAgentList players={players.catchers} label="CATCHERS" />
  <FreeAgentList players={players.firstBasemen} label="FIRST BASEMEN" />
  <FreeAgentList players={players.secondBasemen} label="SECOND BASEMEN" />
  <FreeAgentList players={players.thirdBasemen} label="THIRD BASEMEN" />
  <FreeAgentList players={players.shortstops} label="SHORTSTOPS" />
  <FreeAgentList players={players.outfielders} label="OUTFIELDERS" />
  <FreeAgentList players={players.startingPitchers} label="STARTING PITCHERS" />
  <FreeAgentList players={players.reliefPitchers} label="RELIEF PITCHERS" />
</section>

{#if showAddPlayer}
  <PlayerAdd on:cancel={() => (showAddPlayer = false)} />
{/if}