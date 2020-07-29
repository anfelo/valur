<script>
  export let auth;
  export let user;
</script>

<style>
  .menu {
    height: 100vh;
    width: 200px;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.3);
    z-index: 1;
  }

  .menu-footer {
    margin-top: auto;
    text-align: center;
  }
</style>

<aside class="menu">
  <p class="menu-label">Valuation</p>
  <ul class="menu-list">
    <li>
      <a href="#">Projects</a>
    </li>
    <li>
      <a href="#">Companies</a>
    </li>
  </ul>
  <div class="menu-footer">
    <button class="button is-danger" on:click={() => auth.signOut()}>
      Log Out
    </button>
  </div>
</aside>
