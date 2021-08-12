<script>
  import { createEventDispatcher } from 'svelte';
  import { slide } from 'svelte/transition';
  import InputText from './InputText.svelte';
  import Checkbox from './Checkbox.svelte';
  import Button from './Button.svelte';
  import DividerLabel from './DividerLabel.svelte';
  import { teams } from '../data/teams';

  export let isEdit = false;
  export let prospect = false;
  export let own = false;
  export let available = true;
  export let player = {
    fname: '',
    lname: '',
    team: 'N/A',
    position: '',
    own: own,
    available: available,
    prospect: prospect,
    watchlist: false,
    dropping: false,
    inCollege: false,
    inHighSchool: false,
    rankings: {
      mlb: 0,
      baseballAmerica: 0,
      baseballProspectus: 0,
      fanGraphs: 0,
    },
  };

  const dispatch = createEventDispatcher();

  let hasBeenChanged = false;

  const createPlayer = () => {
    player.position = player.position.split(',');
    if (hasBeenChanged) {
      db.collection('players')
        .add(player)
        .then(() => {
          dispatch('cancel');
        })
        .catch((err) => {
          console.log('Error: ', err);
        });
    }
  };

  const deletePlayer = (player) => {
    if (
      confirm(
        `Are you sure you want to delete ${player.fname} ${player.lname}?`
      )
    ) {
      db.collection('players')
        .doc(player.id)
        .delete()
        .then(() => {
          dispatch('cancel');
        })
        .catch((err) => {
          console.log('Error: ', err);
        });
    }
  };

  const editPlayer = () => {
    if (!Array.isArray(player.position))
      player.position = player.position.split(',');
    if (hasBeenChanged) {
      db.collection('players')
        .doc(player.id)
        .set(player)
        .then(() => {
          dispatch('cancel');
        })
        .catch((err) => {
          console.log('Error: ', err);
        });
    }
    dispatch('cancel');
  };

  const updatePlayer = (event, control) => {
    hasBeenChanged = true;
    player[control] = event.detail;
  };

  const updateRanking = (event, control) => {
    hasBeenChanged = true;
    player.rankings[control] = event.detail;
  };
</script>

<div class="form">
  <div class="form-row">
    <InputText
      label="First name"
      fieldName="fname"
      value={player.fname}
      size="medium"
      autofocus
      on:input={(event) => updatePlayer(event, 'fname')}
    />

    <InputText
      label="Last name"
      fieldName="lname"
      value={player.lname}
      size="medium"
      on:input={(event) => updatePlayer(event, 'lname')}
    />
  </div>

  <div class="form-row">
    <InputText
      label="Position"
      fieldName="position"
      value={player.position}
      size="medium"
      on:input={(event) => updatePlayer(event, 'position')}
    />

    <div class="input-container">
      <label class="label" for="team">Team</label>
      <select
        class="select medium"
        bind:value={player.team}
        on:change={() => (hasBeenChanged = true)}
        id="team"
      >
        {#each teams as team}
          <option value={team}>{team}</option>
        {/each}
      </select>
    </div>
  </div>

  <div class="form-row">
    <Checkbox
      label="Own"
      checked={player.own}
      fieldName="own"
      on:input={(event) => updatePlayer(event, 'own')}
    />

    <Checkbox
      label="Available"
      checked={player.available}
      fieldName="available"
      on:input={(event) => updatePlayer(event, 'available')}
    />
  </div>

  <div class="form-row">
    <Checkbox
      label="Prospect"
      checked={player.prospect}
      fieldName="prospect"
      on:input={(event) => updatePlayer(event, 'prospect')}
    />

    {#if player.own}
      <Checkbox
        label="Dropping"
        checked={player.dropping}
        fieldName="dropping"
        on:input={(event) => updatePlayer(event, 'dropping')}
      />
    {:else}
      <Checkbox
        label="Watchlist"
        checked={player.watchlist}
        fieldName="watchlist"
        on:input={(event) => updatePlayer(event, 'watchlist')}
      />
    {/if}
  </div>

  {#if player.prospect}
    <div class="form-row">
      <Checkbox
        label="In College"
        checked={player.inCollege}
        fieldName="inCollege"
        on:input={(event) => updatePlayer(event, 'inCollege')}
      />

      <Checkbox
        label="In High School"
        checked={player.inHighSchool}
        fieldName="inHighSchool"
        on:input={(event) => updatePlayer(event, 'inHighSchool')}
      />
    </div>

    <div in:slide={{ duration: 100 }} out:slide={{ duration: 100 }}>
      <DividerLabel label="RANKINGS" line />
      <div class="form-row">
        <InputText
          label="MLB"
          type="number"
          fieldName="mlb"
          value={player.rankings.mlb}
          size="small"
          on:input={(event) => updateRanking(event, 'mlb')}
        />

        <InputText
          label="BA"
          type="number"
          fieldName="baseballAmerica"
          value={player.rankings.baseballAmerica}
          size="small"
          on:input={(event) => updateRanking(event, 'baseballAmerica')}
        />

        <InputText
          label="BP"
          type="number"
          fieldName="baseballProspectus"
          value={player.rankings.baseballProspectus}
          size="small"
          on:input={(event) => updateRanking(event, 'baseballProspectus')}
        />

        <InputText
          label="FG"
          type="number"
          fieldName="fanGraphs"
          value={player.rankings.fanGraphs}
          size="small"
          on:input={(event) => updateRanking(event, 'fanGraphs')}
        />
      </div>
    </div>
  {/if}

  <div class="action-buttons">
    <Button type="secondary" on:click={() => dispatch('cancel')}>Cancel</Button>
    <Button on:click={isEdit ? editPlayer : createPlayer}>Save</Button>
    {#if isEdit}
      <Button type="danger" on:click={() => deletePlayer(player)}>Delete</Button
      >
    {/if}
  </div>
</div>
