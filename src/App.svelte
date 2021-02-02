<script>
  import { onMount } from "svelte";

  let news = null;

  const categories = [
    { id: "art", name: "Art" },
    { id: "business", name: "Business" },
    { id: "culture", name: "Culture" },
    { id: "media", name: "Media" },
    { id: "money", name: "Money" },
    { id: "politics", name: "Politics" },
    { id: "science", name: "Science" },
    { id: "tech", name: "Technology" },
    { id: "world", name: "World" },
  ];

  let selectedCategory = categories[8];
  let indexSelected = 8;

  const api = `https://gifthenews-server-3phmh.ondigitalocean.app/`;

  onMount(async () => {
    await fetch(api + selectedCategory.id)
      .then((r) => r.json())
      .then((data) => {
        news = data;
      });
  });

  async function selectCategory(event) {
    news = null;
    await fetch(api + event.srcElement.title)
      .then((r) => r.json())
      .then((data) => {
        news = data;
        selectedCategory = categories[event.srcElement.id];
        indexSelected = event.srcElement.id;
      });
  }
</script>

<main>
  <h1>Gif the News</h1>
  <nav>
    <ul>
      {#each categories as category, i}
        <li class={indexSelected == i ? "active" : ""}>
          <a on:click={selectCategory} id={i} title={category.id}
            >{category.name}</a
          >
        </li>
      {/each}
    </ul>
  </nav>
</main>

{#if news}
  <section class="news">
    {#each news as article}
      <a href={article.url} target="_blank" class="card">
        <article>
          <img src={article.image} class="giphy" alt={article.title} />
          <div class="guardian">
            <h3>{article.title}</h3>
          </div>
        </article>
      </a>
    {/each}
  </section>
{:else}
  <p class="loading">Gipyhing The Guardian...</p>
{/if}

<footer>
  Made in jest by
  <a href="https://twitter.com/armn" target="_blank">armn</a>. Source code
  available at
  <a href="https://github.com/armn/gifthenews/" target="_blank">GitHub</a>.
  <br />

  Made possible by the superb API's of
  <a href="https://developers.giphy.com/" target="_blank">GIPHY</a>
  and
  <a href="https://open-platform.theguardian.com/" target="_blank"
    >The Guardian</a
  >. <br />
  <img style="margin-top: 10px;" src="/giphy.png" alt="Powered by GIPHY" />
</footer>

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
    margin: 0;
  }
  h3 {
    color: black;
    font-size: 1.4em;
    font-weight: 400;
    margin: 0;
  }

  nav {
    display: flex;
    flex-wrap: wrap;
    max-width: 800px;
    margin: 0 auto;
  }

  nav ul {
    list-style-type: none;
    display: flex;
    flex-wrap: wrap;
  }

  nav ul li {
    cursor: pointer;
    color: black;
    margin: 0 15px;
    font-weight: bold;
  }

  li.active {
    border-bottom: 2px solid black;
  }

  .card {
    width: 400px;
    margin: 20px;
    border-radius: 50px;
    /* background: #f5f5f5; */
    /* border: 1px solid #e7e7e7; */
    /* box-shadow: 20px 20px 82px #e2e2e2, -20px -20px 82px #ffffff; */
    display: flex;
    flex-direction: column;
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
    max-width: 320px;
    text-align: center;
  }
  .giphy {
    border-radius: 50px;
    height: 300px;
    margin: 15px auto;
    display: block;
    max-width: calc(100% - 30px);
  }
  .guardian {
    padding: 10px 30px 30px 30px;
  }

  footer {
    text-align: center;
    margin-top: 30px;
    padding: 15px;
  }
</style>
