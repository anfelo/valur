<script>
  import { Doc, Collection } from 'sveltefire';
  import ProjectCard from './ProjectCard.svelte';
  import CreateProjects from './CreateProjects.svelte';
  import ProjectDetail from './ProjectDetail.svelte';

  export let user;

  let currentProject;
  let isEditing;

  function handleCreateOrEdit(project) {
    currentProject = project || getNewProject();
    isEditing = true;
  }

  function closeDetail() {
    currentProject = {};
    isEditing = false;
  }

  function getNewProject() {
    return {
      title: '',
      description: '',
      type: '',
      years: 0,
      discountRate: 0,
      cashflows: [],
    };
  }
</script>

<style>
  .projects-container {
    position: relative;
    flex: 1;
    min-height: 100vh;
    padding: 1rem;
  }

  .projects-list {
    display: flex;
  }

  .create-option {
    width: 119px;
    height: 119px;
    border-style: dashed;
    border-color: rgba(0, 0, 0, 0.5);
    border-radius: 6px;
    box-shadow: 0 0.5em 1em -0.125em rgba(10, 10, 10, 0.1),
      0 0 0 1px rgba(10, 10, 10, 0.02);
    color: white;
    cursor: pointer;
  }

  .project-detail-container {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 0;
    height: 0;
    transition: all 0.3s ease-out;
    overflow: hidden;
  }

  .project-detail-container.is-active {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    padding: 1rem;
  }
</style>

<div class="projects-container">
  <Doc
    path={`projects/${user.uid}`}
    let:data={projects}
    let:ref={projectsRef}
    log>

    <span slot="fallback">
      <CreateProjects docRef={projectsRef} />
    </span>

    <h2 class="title">{projects.title}</h2>
    <hr />
    <span slot="loading">Loading projects...</span>

    <Collection
      path={projectsRef.collection('options')}
      query={(ref) => ref.orderBy('createdAt')}
      let:data={options}
      let:ref={optionsRef}
      log>

      {#if !options.length}No investment projects yet...{/if}

      <div class="projects-list">
        {#each options as option}
          <div on:click={() => handleCreateOrEdit(option)}>
            <ProjectCard {option} />
          </div>
        {/each}
        <button
          class="create-option has-background-primary"
          on:click={() => handleCreateOrEdit(null)}>
          <h3 class="title">➕</h3>
        </button>
        <div
          class="project-detail-container has-background-light {isEditing ? 'is-active' : ''}">
          {#if isEditing}
            <ProjectDetail
              project={currentProject}
              collRef={optionsRef}
              docRef={currentProject.ref}
              on:close={closeDetail} />
          {/if}
        </div>
      </div>

      <span slot="loading">Loading investment projects...</span>

    </Collection>
  </Doc>
</div>
