<script>
  export let option;
</script>

<style>
  .box {
    margin-right: 15px;
  }

  .box:not(:last-child) {
    margin-bottom: 0;
  }
</style>

<div class="box">
  <article class="media">
    <div class="media-content">
      <div class="content">
        <p class="title is-4">{option.title}</p>

      </div>
      <nav class="level is-mobile">
        <div class="level-left">
          <button
            class="button is-warning"
            on:click={() => option.ref.delete()}>
            Delete
          </button>
        </div>
      </nav>
    </div>
  </article>
</div>
