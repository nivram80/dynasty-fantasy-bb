<script>
  import InputText from './InputText.svelte';
  import Button from './Button.svelte';

  export let user = { email: '', password: '' };

  const signIn = () => {
    auth.signInWithEmailAndPassword(user.email, user.password)
      .then((resp) => {
        user = resp.user;
      })
      .catch((error) => {
        console.error(error.code, error.message);
      });
  }

  const updatePlayer = (event, control) => {
    user[control] = event.detail;
  }
</script>

<div class="form">
  <InputText
    label="Email"
    fieldName="email"
    value={user.email}
    size="large"
    autofocus
    on:input={event => updatePlayer(event, 'email')} />

  <InputText
    label="Password"
    fieldName="password"
    value={user.password}
    size="large"
    type="password"
    on:input={event => updatePlayer(event, 'password')} />
  <div class="action-buttons">
    <Button on:click={() => signIn()}>Log In</Button>
  </div>
</div>