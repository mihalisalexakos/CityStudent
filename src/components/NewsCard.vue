<template>
    <div class="news-card" v-if="articles.length">
      <h2 style="color: var(--black)">News</h2>
      <ul>
        <li v-for="article in articles" :key="article.url" class="news-item">
          <a :href="article.url" target="_blank">
            <strong><i class="fa-solid fa-newspaper"></i> {{ article.title }}</strong>
          </a>
          <p>{{ article.description }}</p>
          <small>{{ article.source.name }} – {{ formatDate(article.publishedAt) }}</small>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: ['articles'],
    methods: {
      formatDate(dateStr) {
        return new Date(dateStr).toLocaleString()
      }
    }
  }
  </script>
  
  <style scoped>
  .news-card {
    background-color: var(--white);
    outline: 2px solid var(--element);
    padding: 10px;
    border-radius: 5px;
    box-sizing: border-box;

    display: flex;
    flex-direction: column;
    align-items: center;
    max-height: 600px;
    color: var(--blueDK);
    overflow: auto;
  
  }
  .news-item {
    margin-bottom: 1rem;
  }
  .news-item a {
    color: var(--blue);
    text-decoration: none;
  }

  .news-card ul{
    list-style-type: none;
    padding: 0px;
    margin: 0px;
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    gap: 15px;
    width: 100%;
    align-items: center;
    justify-content: center;
  }

  .news-card li{

    border: 2px solid var(--element);
    padding: 10px;
    width: 90%;
    border-radius: 5px;
    box-sizing: border-box;

  }
  </style>
  