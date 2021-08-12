<script>
  import { onMount, onDestroy } from 'svelte';
  import IconPlus from './IconPlus.svelte';
  import PlayerList from './PlayerList.svelte';
  import PlayerAdd from './PlayerAdd.svelte';

  let players = {
    catchers: [],
    firstBasemen: [],
    secondBasemen: [],
    thirdBasemen: [],
    shortstops: [],
    outfielders: [],
  };
  let showAddPlayer = false;
  let unsubscribeCatchers;
  let unsubscribeFirstBasemen;
  let unsubscribeSecondBasemen;
  let unsubscribeThirdBasemen;
  let unsubscribeShortstops;
  let unsubscribeOutfielders;

  onMount(() => {
    getCatchers();
    getFirstBasemen();
    getSecondBasemen();
    getThirdBasemen();
    getShortstops();
    getOutfielders();
  });

  onDestroy(() => {
    unsubscribeCatchers();
    unsubscribeFirstBasemen();
    unsubscribeSecondBasemen();
    unsubscribeThirdBasemen();
    unsubscribeShortstops();
    unsubscribeOutfielders();
  });

  const moveProspectsToEnd = (players) => {
    let mlbPlayers = [];
    let prospects = [];
    players.forEach((p) => {
      if (p.prospect) {
        prospects.push(p);
      } else {
        mlbPlayers.push(p);
      }
    });
    let sortedPlayers = [...mlbPlayers, ...prospects];
    return sortedPlayers;
  }

  const getCatchers = () => {
    unsubscribeCatchers = db
      .collection('players')
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains', 'C')
      .onSnapshot((snapshot) => {
        players['catchers'] = snapshot.docs.map((doc) => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        players['catchers'] = moveProspectsToEnd(players['catchers']);
      });
  };

  const getFirstBasemen = () => {
    unsubscribeFirstBasemen = db
      .collection('players')
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains', '1B')
      .onSnapshot((snapshot) => {
        players['firstBasemen'] = snapshot.docs.map((doc) => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        players['firstBasemen'] = moveProspectsToEnd(players['firstBasemen']);
      });
  };

  const getSecondBasemen = () => {
    unsubscribeSecondBasemen = db
      .collection('players')
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains', '2B')
      .onSnapshot((snapshot) => {
        players['secondBasemen'] = snapshot.docs.map((doc) => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        players['secondBasemen'] = moveProspectsToEnd(players['secondBasemen']);
      });
  };

  const getThirdBasemen = () => {
    unsubscribeThirdBasemen = db
      .collection('players')
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains', '3B')
      .onSnapshot((snapshot) => {
        players['thirdBasemen'] = snapshot.docs.map((doc) => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        players['thirdBasemen'] = moveProspectsToEnd(players['thirdBasemen']);
      });
  };

  const getShortstops = () => {
    unsubscribeShortstops = db
      .collection('players')
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains', 'SS')
      .onSnapshot((snapshot) => {
        players['shortstops'] = snapshot.docs.map((doc) => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        players['shortstops'] = moveProspectsToEnd(players['shortstops']);
      });
  };

  const getOutfielders = () => {
    unsubscribeOutfielders = db
      .collection('players')
      .where('available', '==', false)
      .where('own', '==', true)
      .where('dropping', '==', false)
      .where('position', 'array-contains-any', ['OF', 'LF', 'CF', 'RF'])
      .onSnapshot((snapshot) => {
        players['outfielders'] = snapshot.docs.map((doc) => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        players['outfielders'] = moveProspectsToEnd(players['outfielders']);
      });
  };
</script>

<section class="page-column small">
  <header>
    <h3>Keepers - Positions</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <PlayerList players={players.catchers} label="CATCHERS" />
  <PlayerList players={players.firstBasemen} label="FIRST BASEMEN" />
  <PlayerList players={players.secondBasemen} label="SECOND BASEMEN" />
  <PlayerList players={players.thirdBasemen} label="THIRD BASEMEN" />
  <PlayerList players={players.shortstops} label="SHORTSTOPS" />
  <PlayerList players={players.outfielders} label="OUTFIELDERS" />
</section>

{#if showAddPlayer}
  <PlayerAdd on:cancel={() => (showAddPlayer = false)} own available={false} />
{/if}
