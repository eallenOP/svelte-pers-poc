<script>
import { get } from 'svelte/store';
import { afterUpdate } from 'svelte';

    // The Person component gets the individual and displays their id and name
    import Person from './Person.svelte'

    // Prop to specify which team to get
   export let teamNumber;
   
   // Free API for testing: https://reqres.in/
   let endpoint = `https://reqres.in/api/users?page=${teamNumber}`;

    // Doing it this way in order to use await block below
    async function getTeam(url) {
      const response = await fetch(url, {
        // Obvious, but here to remind me how to include auth token in future
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      });
      const team = await response.json();
  
      // Not sure if this is doing anything useful at the moment. Comes from https://svelte.dev/tutorial/await-blocks
      if (response.ok) {
        return team;
      } else {
        throw new Error(team);
      }
    }
    
    afterUpdate(() => {
      // ...the DOM is now in sync with the data
      endpoint = `https://reqres.in/api/users?page=${teamNumber}`;
    });
    
      // Reveals the data to the await block below (reacts when endpoint changes)
      $: promise = getTeam(endpoint);
  </script>

  {#await promise}
  <!-- Can put anything here to tell people data is incoming (super-handy!) -->
    <p>...waiting</p>
  {:then team}
  <!-- What the component will display when data is loaded -->
    <p>The team you have fetched is Team {team.page}</p>

    <div class="team">
        <ul>
        {#each team.data as per}
        <!-- Loop over the array of team members (list users https://reqres.in/) -->
            <li>
                <!-- Pass the id into the Person component, which will display the id and name -->
                <Person id={per.id} />
            </li>
        {/each}
        </ul>
    </div>
    

  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
  
  <!-- Scoped styles are so hot right now -->
  <style>
    ul {
        list-style-type: none;
        text-align: left;
        padding: 0 1em;
        margin: 0;
    }
    .team {
        max-width: 200px;
        margin: 0 auto;
        border: 1px solid grey;
    }
  </style>