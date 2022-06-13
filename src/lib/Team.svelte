<script>
    import Person from './Person.svelte'
    export let id;
    const endpoint = `https://reqres.in/api/users?page=${id}`;
  
    async function getTeam() {
      const response = await fetch(endpoint, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      });
      const team = await response.json();
  
      if (response.ok) {
        return team;
      } else {
        throw new Error(team);
      }
    }
  
    let promise = getTeam();

    console.log(promise);
  </script>
  
  {#await promise}
    <p>...waiting</p>
  {:then team}
    <p>The team you have fetched is Team {team.page}</p>

    <div class="team">
        <ul>
        {#each team.data as per}
            <li>
                <Person id={per.id} />
            </li>
        {/each}
        </ul>
    </div>
    

  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
  
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