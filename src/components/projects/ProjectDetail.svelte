<script>
  import { createEventDispatcher } from 'svelte';

  import ProjectCashflows from './ProjectCashFlows.svelte';

  export let collRef;
  export let docRef;
  export let project;

  const dispatch = createEventDispatcher();

  function handleClose() {
    dispatch('close', null);
  }

  function handleSave() {
    if (!project.id) {
      collRef.add({ ...project, createdAt: Date.now() });
    } else {
      docRef.update({ ...project, updatedAt: Date.now() });
    }
    handleClose();
  }

  function handleYearsChange(event) {
    let cashflows = [];
    const value = +event.target.value;
    for (let index = 0; index < value; index++) {
      cashflows = [
        ...cashflows,
        {
          positiveCF: 0,
          negativeCF: 0,
        },
      ];
    }
    project.cashflows = cashflows;
    project.years = value;
  }
</script>

<div class="container is-fluid">
  <button class="button is-pulled-right" on:click={handleClose}>✖</button>
  {#if project.id}
    <h2 class="title">Edit Project</h2>
  {:else}
    <h2 class="title">New Project</h2>
  {/if}

  <div class="field">
    <label class="label">Title</label>
    <div class="control">
      <input
        class="input"
        type="text"
        placeholder="Project title..."
        bind:value={project.title} />
    </div>
  </div>

  <div class="field">
    <label class="label">Description</label>
    <div class="control">
      <input
        class="input"
        type="text"
        placeholder="Some awesome project..."
        bind:value={project.description} />
    </div>
  </div>

  <div class="field">
    <label class="label">Type</label>
    <div class="control">
      <div class="select">
        <select bind:value={project.type}>
          <option>Select project type</option>
          <option value="bond">Bond</option>
        </select>
      </div>
    </div>
  </div>

  <div class="field">
    <label class="label">Years</label>
    <div class="control">
      <input
        class="input"
        type="number"
        max="30"
        value={project.years}
        on:input={handleYearsChange} />
    </div>
  </div>

  <div class="field">
    <label class="label">Discount Rate</label>
    <div class="control">
      <input class="input" type="number" bind:value={project.discountRate} />
    </div>
  </div>

  <div class="field">
    <label class="label">Cash Flows</label>
    {#if !project.years || project.years < 2}
      <p>Set a number of years</p>
    {:else}
      <ProjectCashflows bind:cashflows={project.cashflows} />
    {/if}
  </div>

  <button class="button is-primary" on:click={handleSave}>
    {#if project.id}Save Project{:else}Create Project{/if}
  </button>
</div>
