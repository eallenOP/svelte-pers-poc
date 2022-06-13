<script>
  export let id;
  const endpoint = `https://reqres.in/api/users/${id}`;

  async function getPerson() {
    const response = await fetch(endpoint, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
      },
    });
    const person = await response.json();

    if (response.ok) {
      return person.data;
    } else {
      throw new Error(person);
    }
  }

  let promise = getPerson();
</script>

{#await promise}
  <p>...waiting</p>
{:then person}
  <p>{person.id}: {person.first_name} {person.last_name}</p>
{:catch error}
  <p style="color: red">{error.message}</p>
{/await}
