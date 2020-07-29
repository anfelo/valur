<script>
  import { FirebaseApp, User, Doc, Collection } from 'sveltefire';
  import firebase from 'firebase/app';
  import 'firebase/firestore';
  import 'firebase/auth';
  import 'firebase/performance';
  import 'firebase/analytics';
  import * as fromFirebase from './firebase-config';

  // Components
  import Login from './components/Login.svelte';
  import Menu from './components/Menu.svelte';
  import Projects from './components/projects/Projects.svelte';

  firebase.initializeApp(fromFirebase.firebaseConfig);
</script>

<style>
  .home-container {
    display: flex;
  }
</style>

<main>
  <FirebaseApp {firebase}>

    <User let:user let:auth>
      <div slot="signed-out">
        <Login {auth} />
      </div>

      <div class="home-container">
        <Menu {auth} {user} />
        <Projects {user} />
      </div>
    </User>
  </FirebaseApp>
</main>
