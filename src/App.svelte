<script>
  import { onMount } from "svelte";
  import Item from "./movie/Item.svelte";

  const APIKEY = "00d23a7ba8835c665646fd7a63645408";
  const BASEURL = "https://api.themoviedb.org/3";
  const APISETTINGS = `?api_key=${APIKEY}&language=es-ES`;

  let movies = [];

  function getMovies() {
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`;
    fetch(URL)
      .then(response => response.json())
      .then(({ results }) => {
        movies = results;
      });
  }

  onMount(() => {
    console.log("the component has mounted");
    getMovies();
  });
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  /*   h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  } */

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<svelte:head>
  <title>Svelte Movie App</title>
  <link
    rel="stylesheet"
    href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
    integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"
    crossorigin="anonymous" />
</svelte:head>

<main>
  <div class="container">
    <div class="row">
      {#each movies as movie}
        <div class="col-12 col-md-6 col-lg-3 p-2">
          <Item />
        </div>
      {/each}
    </div>
  </div>
</main>
