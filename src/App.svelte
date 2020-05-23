<script>
  /* import { onMount } from "svelte"; */
  import Item from "./movie/Item.svelte";

  const APIKEY = "00d23a7ba8835c665646fd7a63645408";
  const BASEURL = "https://api.themoviedb.org/3";
  const APISETTINGS = `?api_key=${APIKEY}&language=es-ES`;

  /* let movies = [];

  function getMovies() {
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`;
    fetch(URL)
      .then(response => response.json())
      .then(({ results }) => {
        movies = results;
      });
  } */

  const getMovies = (async () => {
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`;
    const response = await fetch(URL);
    return await response.json();
  })();

  /* onMount(() => {
    console.log("the component has mounted");
    getMovies();
  }); */

  let favorites = [];

  const handleClick = event => {
    const movie = event.detail;
    let index = favorites.findIndex(element => element.id === movie.id);

    if (index >= 0) {
      favorites.splice(index, 1);
    } else {
      favorites.push(movie);
    }

    favorites = favorites;
    console.log(favorites);
  };
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

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
      <div class="col-12 col-md-6 col-lg-8">
        <h1>Peliculas Populares</h1>
        <!-- {#each movies as movie}
        <div class="col-12 col-md-6 col-lg-3 p-2">
          <Item
            id={movie.id}
            title={movie.title}
            overview={movie.overview}
            posterPath={movie.poster_path} />
        </div>
      {/each} -->
        <div class="row">
          {#await getMovies}
            <!-- promise is pending -->
            <div class="col-12">
              <p>Cargando información...</p>
            </div>
          {:then data}
            {#each data.results as movie}
              <div class="col-12 col-md-6 col-lg-4 p-2">
                <Item
                  id={movie.id}
                  title={movie.title}
                  overview={movie.overview}
                  posterPath={movie.poster_path}
                  on:onHandleClickLike={handleClick} />
              </div>
            {/each}
          {/await}
        </div>
      </div>
      <div class="col-12 col-md-6 col-lg-4">
        <h2>Peliculas Favoritas</h2>
        <div class="row">
          {#if favorites.length}
            {#each favorites as movie}
              <div class="col-12 col-md-6 col-lg-4 p-2">
                <Item
                  id={movie.id}
                  title={movie.title}
                  overview=""
                  posterPath={movie.poster_path}
                  on:onHandleClickLike={handleClick} />
              </div>
            {/each}
          {:else}
            <div class="col-12">
              <p>No hay peliculas favoritas</p>
            </div>
          {/if}
        </div>
      </div>
    </div>
  </div>
</main>
