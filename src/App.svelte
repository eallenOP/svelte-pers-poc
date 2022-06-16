<script>
  import logo from './assets/svelte.png'
  import Team from './lib/Team.svelte'

  let teamNumber = 1;

</script>

<main>
  <img src={logo} alt="Svelte Logo" />
  <h1>Hello world!</h1>

  <form on:submit|preventDefault>
    <label for="teamNo">Which team number to display (there are only 2 in this data set):</label>
    <input type="number" id="teamNo" name="teamNo" bind:value={teamNumber} min="1" max="2" >
  </form>

  <p>{teamNumber}</p>
  <Team {teamNumber} />

</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  img {
    height: 16rem;
    width: 16rem;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  p {
    max-width: 14rem;
    margin: 1rem auto;
    line-height: 1.35;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }

    p {
      max-width: none;
    }
  }
</style>
