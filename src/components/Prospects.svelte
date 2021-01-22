<script>
  import { onMount, onDestroy } from "svelte";
  import PlayerAdd from "./PlayerAdd.svelte";
  import PlayerEdit from "./PlayerEdit.svelte";
  import IconPlus from "./IconPlus.svelte";
  import IconEdit from "./IconEdit.svelte";
  import ProspectList from "./ProspectList.svelte";

  let highSchoolProspects = [];
  let collegiateProspects = [];
  let proProspects = [];
  let selectedPlayer = null;
  let showAddPlayer = false;
  let showEditPlayer = false;
  let unsubscribeCollegiates;
  let unsubscribeHighSchoolers;
  let unsubscribePros;

  onMount(() => getPlayers());

  onDestroy(() => {
    unsubscribeCollegiates();
    unsubscribeHighSchoolers();
    unsubscribePros();
  });

  const displayEditForm = player => {
    selectedPlayer = player;
    showEditPlayer = true;
  };

  const getPlayers = () => {
    unsubscribeCollegiates = db.collection("players")
      .where("prospect", '==', true)
      .where('available', '==', true)
      .where('inHighSchool', '==', true)
      .onSnapshot(snapshot => {
        highSchoolProspects = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        highSchoolProspects = sortPlayers(highSchoolProspects, "baseballAmerica");
    });

    unsubscribeHighSchoolers = db.collection("players")
      .where("prospect", '==', true)
      .where('available', '==', true)
      .where('inCollege', '==', true)
      .onSnapshot(snapshot => {
        collegiateProspects = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        collegiateProspects = sortPlayers(collegiateProspects, "baseballAmerica");
    });

    unsubscribePros = db.collection("players")
      .where("prospect", '==', true)
      .where('available', '==', true)
      .where('inHighSchool', '==', false)
      .where('inCollege', '==', false)
      .onSnapshot(snapshot => {
        proProspects = snapshot.docs.map(doc => {
          let player = doc.data();
          player.id = doc.id;
          return player;
        });
        proProspects = sortPlayers(proProspects, "baseballAmerica");
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

  const sortPlayers = (players, column) => {
    // Based on https://stackoverflow.com/a/29829370.
    return players.sort((a, b) => {
      return (
        (a.rankings[column] === 0) - (b.rankings[column] === 0) ||
        +(a.rankings[column] > b.rankings[column]) ||
        -(a.rankings[column] < b.rankings[column])
      );
    });
  };
</script>

<section class="page-column large">
  <ProspectList 
    prospects={proProspects}
    listTitle="Available Pro Prospects"
    on:showAddPlayer={() => (showAddPlayer = true)}
    on:displayEditForm={(e) => displayEditForm(e.detail)}
    on:sortPlayers={(e) => proProspects = sortPlayers(proProspects, e.detail)} />

  <ProspectList 
    prospects={collegiateProspects}
    listTitle="Available College Prospects"
    on:showAddPlayer={() => (showAddPlayer = true)}
    on:displayEditForm={(e) => displayEditForm(e.detail)}
    on:sortPlayers={(e) => collegiateProspects = sortPlayers(collegiateProspects, e.detail)} />

  <ProspectList 
    prospects={highSchoolProspects}
    listTitle="Available High School Prospects"
    on:showAddPlayer={() => (showAddPlayer = true)}
    on:displayEditForm={(e) => displayEditForm(e.detail)}
    on:sortPlayers={(e) => highSchoolProspects = sortPlayers(highSchoolProspects, e.detail)} />

</section>

{#if showAddPlayer}
  <PlayerAdd prospect on:cancel={() => (showAddPlayer = false)} />
{/if}

{#if showEditPlayer}
  <PlayerEdit
    player={selectedPlayer}
    on:cancel={() => (showEditPlayer = false)} />
{/if}
