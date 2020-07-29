<script>
  export let docRef;
</script>

<section class="hero is-light is-fullheight">
  <div class="hero-body">
    <div class="container has-text-centered">
      <button
        class="button is-primary is-large"
        on:click={() => docRef.set({
            title: '🎢 My Projects',
            createdAt: Date.now(),
          })}>
        Create Projects Portfolio
      </button>
    </div>
  </div>
</section>
