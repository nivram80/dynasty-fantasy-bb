<script>
  import { createEventDispatcher } from 'svelte';
  import Dialog from './Dialog.svelte';
  import Card from './Card.svelte';
  import InputText from './InputText.svelte';
  import Checkbox from './Checkbox.svelte';
  import Button from './Button.svelte';
  import DividerLabel from './DividerLabel.svelte';

  const dispatch = createEventDispatcher();

  let player = {
    fname: '',
    lname: '',
    team: '',
    position: '',
    prospect: false,
    watchlist: false,
    inCollege: false,
    inHighSchool: false,
    rankings: {
      mlb: 0,
      baseballAmerica: 0,
      baseballProspectus: 0,
    },
  };

  const createPlayer = () => {
    console.log('player: ', player);
    db.collection('players').add(player)
    .then(() => {
      dispatch('cancel');
    })
    .catch((err) => {
      console.log('Error: ', err);
    })
  }

  const updatePlayer = (event, control) => {
    player[control] = event.detail;
  }

  const updateRanking = (event, control) => {
    player.rankings[control] = event.detail;
  }
</script>

<Dialog>
  <Card>
    <h3>Add Player</h3>
    <div class="form">
      <div class="form-row">
        <InputText
          label="First name"
          fieldName="fname"
          value={player.fname}
          size="medium"
          autofocus
          on:input={event => updatePlayer(event, 'fname')} />

        <InputText
          label="Last name"
          fieldName="lname"
          value={player.lname}
          size="medium"
          on:input={event => updatePlayer(event, 'lname')} />
      </div>

      <div class="form-row">
        <InputText
          label="Position"
          fieldName="position"
          value={player.position}
          size="medium"
          on:input={event => updatePlayer(event, 'position')} />

        <InputText
          label="Team"
          fieldName="team"
          value={player.team}
          size="medium"
          on:input={event => updatePlayer(event, 'team')} />
      </div>

      <div class="form-row">
        <Checkbox 
          label="Prospect"
          checked={player.prospect}
          fieldName="prospect"
          on:input={event => updatePlayer(event, 'prospect')} />

        <Checkbox 
          label="Watchlist"
          checked={player.watchlist}
          fieldName="watchlist"
          on:input={event => updatePlayer(event, 'watchlist')} />
      </div>

      <div class="form-row">
        <Checkbox 
          label="In College"
          checked={player.inCollege}
          fieldName="inCollege"
          on:input={event => updatePlayer(event, 'inCollege')} />

        <Checkbox 
          label="In High School"
          checked={player.inHighSchool}
          fieldName="inHighSchool"
          on:input={event => updatePlayer(event, 'inHighSchool')} />
      </div>

      <DividerLabel label="RANKINGS" />

      <div class="form-row">
        <InputText
          label="MLB"
          type="number"
          fieldName="mlb"
          value={player.rankings.mlb}
          size="small"
          autofocus
          on:input={event => updateRanking(event, 'mlb')} />

        <InputText
          label="BA"
          type="number"
          fieldName="baseballAmerica"
          value={player.rankings.baseballAmerica}
          size="small"
          on:input={event => updateRanking(event, 'baseballAmerica')} />
      
        <InputText
          label="BP"
          type="number"
          fieldName="baseballProspectus"
          value={player.rankings.baseballProspectus}
          size="small"
          on:input={event => updateRanking(event, 'baseballProspectus')} />
      </div>

      <div class="action-buttons">
        <Button type="secondary" on:click={() => dispatch('cancel')}>Cancel</Button>
        <Button on:click={createPlayer}>Save</Button>
      </div>
      
    </div>
  </Card>
</Dialog>

<style>
  h3 {
    text-align: center;
  }
</style>