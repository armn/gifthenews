<script>
  import { onMount } from "svelte";

  export let news;

  onMount(async () => {
    await fetch(`https://gifthenews-server-3phmh.ondigitalocean.app`)
      .then((r) => r.json())
      .then((data) => {
        news = data;
      });
  });
</script>

<main>
  <h1>Gif the News</h1>
</main>

{#if news}
  <section class="news">
    {#each news as article}
      <a href={article.url} target="_blank">
        <article>
          <img src={article.image} class="giphy" alt={article.title} />
          <div class="guardian">
            <h2>{article.title}</h2>
          </div>
        </article>
      </a>
    {/each}
  </section>
{:else}
  <p class="loading">Gipyhing The Guardian...</p>
{/if}

<style>
  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  h1 {
    color: #8400ff;
    font-size: 4em;
    font-weight: 100;
  }

  h2 {
    color: #140703;
    font-size: 1.4em;
    font-weight: 400;
    margin: 0;
  }

  .news {
    display: flex;
    max-width: 1400px;
    margin: 0 auto;
    flex-wrap: wrap;
    justify-content: center;
  }

  .loading {
    margin: auto;
    font-size: 2em;
    max-width: 300px;
    text-align: center;
  }

  .news article {
    width: 400px;
    margin: 20px;
    border-radius: 50px;
    background: #e0e0e0;
    box-shadow: 41px 41px 82px #bebebe, -41px -41px 82px #ffffff;
    display: flex;
    flex-direction: column;
  }

  .giphy {
    border-radius: 15px 15px 0 0;
    overflow: hidden;
    height: 300px;
    object-fit: cover;
  }

  .guardian {
    padding: 10px 30px 30px 30px;
  }
</style>
