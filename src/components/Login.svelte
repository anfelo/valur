<script>
  export let auth;

  let email = '';
  let password = '';
  let newUser = false;
  let loading = false;
  let errorMessage;

  async function signInOrCreateUser() {
    loading = true;
    try {
      if (newUser) {
        await auth.createUserWithEmailAndPassword(email, password);
      } else {
        await auth.signInWithEmailAndPassword(email, password);
      }
    } catch (error) {
      errorMessage = error.message;
    }
    loading = false;
  }
</script>

<style>
  .login-wrapper {
    height: 100vh;
    width: 50%;
    margin: auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .is-danger {
    color: tomato;
  }
  .form-link {
    align-self: flex-end;
  }
</style>

<div class="container">
  <form class="login-wrapper">
    {#if newUser}
      <h3 class="title">Sign Up for New Account</h3>
      <a class="form-link" href="#" on:click={() => (newUser = false)}>
        Returning User?
      </a>
    {:else}
      <h3 class="title">Sign In with Email</h3>
      <a class="form-link" href="#" on:click={() => (newUser = true)}>
        New User?
      </a>
    {/if}
    <div class="field">
      <label class="label" for="email">Your Email</label>
      <div class="control">
        <input
          class="input"
          type="email"
          placeholder="test@mailbox.com"
          bind:value={email} />
      </div>
    </div>
    <div class="field">
      <label class="label" for="password">Secret Password</label>
      <div class="control">
        <input
          class="input"
          type="password"
          placeholder="******"
          bind:value={password} />
      </div>
    </div>
    <div class="field is-grouped">
      <div class="control">
        <button
          disabled={loading}
          type="button"
          class={`button is-link ${loading ? 'is-loading' : ''}`}
          on:click={signInOrCreateUser}>
          {newUser ? 'Sign Up' : 'Sign In'}
        </button>
      </div>
      <div class="control">
        <button
          disabled={loading}
          type="button"
          class={`button is-link is-light ${loading ? 'is-loading' : ''}`}
          on:click={() => auth.signInAnonymously()}>
          Continue Anonymously
        </button>
      </div>
    </div>
    {#if errorMessage}
      <p class="is-danger">{errorMessage}</p>
    {/if}
  </form>
</div>
