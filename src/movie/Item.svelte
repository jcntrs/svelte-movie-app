<script>
  import { createEventDispatcher } from "svelte";
  export let id;
  export let title;
  export let overview;
  export let posterPath;

  $: coverURL = `https://image.tmdb.org/t/p/w185_and_h278_bestv2${posterPath}`;

  const dispatch = createEventDispatcher();

  const handleClickLike = () => {
    dispatch("onHandleClickLike", { id, title, overview, posterPath });
  };
</script>

<div class="card">
  <img src={coverURL} class="card-img-top" alt="..." />
  <div class="card-body">
    <h5 class="card-title">{title}</h5>
    <p class="card-text">{overview}</p>
    <div class="text-center">
      <button class="btn btn-primary" on:click={handleClickLike}>Like</button>
    </div>
  </div>
</div>
