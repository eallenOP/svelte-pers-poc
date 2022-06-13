<script>
  // Prop to specify which user to get
  export let id;

  // Free API for testing: https://reqres.in/
  const endpoint = `https://reqres.in/api/users/${id}`;

  // Doing it this way in order to use await block below
  async function getPerson() {
    const response = await fetch(endpoint, {
      // Obvious, but here to remind me how to include auth token in future
      method: "GET",
      headers: {
        "Content-Type": "application/json",
      },
    });
    const person = await response.json();

	// Not sure if this is doing anything useful at the moment. Comes from https://svelte.dev/tutorial/await-blocks
    if (response.ok) {
      return person.data;
    } else {
      throw new Error(person);
    }
  }

  // Reveals the data to the await block below
  let promise = getPerson();
</script>

{#await promise}
<!-- Can put anything here to tell people data is incoming (super-handy!) -->
  <p>Loading person...</p>
{:then person}
<!-- What the component will display when data is loaded -->
  <p><strong>{person.id}:</strong> {person.first_name} {person.last_name}</p>
{:catch error}
  <p style="color: red">{error.message}</p>
{/await}
