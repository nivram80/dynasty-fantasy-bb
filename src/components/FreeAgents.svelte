<script>
  import { onMount, onDestroy } from "svelte";
  import IconPlus from "./IconPlus.svelte";
  import FreeAgentList from "./FreeAgentList.svelte";
  import PlayerAdd from "./PlayerAdd.svelte";

  let catchers = [];
  let firstBasemen = [];
  let secondBasemen = [];
  let thirdBasemen = [];
  let shortstops = [];
  let outfielders = [];
  let startingPitchers = [];
  let reliefPitchers = [];
  
  let players = [];
  let showAddPlayer = false;
  let unsubscribe;

  onMount(() => getPlayers());

  onDestroy(() => unsubscribe());

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
        organizePlayers(players);
      });
  };

  const organizePlayers = (players) => {
    players.forEach((p) => {
      switch (p.position) {
        case 'C':
          catchers = [...catchers, p];
          break;
        case '1B':
          firstBasemen = [...firstBasemen, p];
          break;
        case '2B':
          secondBasemen = [...secondBasemen, p];
          break;
        case '3B':
          thirdBasemen = [...thirdBasemen, p];
          break;
        case 'SS':
          shortstops = [...shortstops, p];
          break;
        case 'OF':
        case 'LF':
        case 'CF':
        case 'RF':
          outfielders = [...outfielders, p];
          break;
        case 'SP':
          startingPitchers = [...startingPitchers, p];
          break;
        case 'RP':
          reliefPitchers = [...reliefPitchers, p];
          break;
      }
    });
  }
</script>

<section class="page-column small">
  <header>
    <h3>Free Agent Watch</h3>
    <div class="action-icon" on:click={() => (showAddPlayer = true)}>
      <IconPlus />
    </div>
  </header>

  <FreeAgentList players={catchers} label="CATCHERS" />
  <FreeAgentList players={firstBasemen} label="FIRST BASEMEN" />
  <FreeAgentList players={secondBasemen} label="SECOND BASEMEN" />
  <FreeAgentList players={thirdBasemen} label="THIRD BASEMEN" />
  <FreeAgentList players={shortstops} label="SHORTSTOPS" />
  <FreeAgentList players={outfielders} label="OUTFIELDERS" />
  <FreeAgentList players={startingPitchers} label="STARTING PITCHERS" />
  <FreeAgentList players={reliefPitchers} label="RELIEF PITCHERS" />
</section>

{#if showAddPlayer}
  <PlayerAdd on:cancel={() => (showAddPlayer = false)} />
{/if}