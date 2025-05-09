<template>
    <div class="wiki-card" v-if="summary">
      <h2>About</h2>
      <img v-if="summary.thumbnail" :src="summary.thumbnail.source" :alt="summary.title" />
      <p>{{ summary.extract }}</p>
      <a :href="summary.content_urls.desktop.page" target="_blank">Read more on Wikipedia</a>
    </div>
  </template>
  
  <script>
  export default {
    props: ['summary']
  }
  </script>
  
  <style scoped>
  .wiki-card {
    padding: 10px;
    background-color: var(--white);
    outline: 2px solid var(--element);
    border-radius: 5px;
    box-sizing: border-box;

    display: flex;
    flex-direction: column;
    align-items: center;
    max-height: 500px;
    color: var(--blueDK);
    overflow: auto;
  }
  img {
    max-width: 95%;
    border-radius: 5px;
    margin-bottom: 1rem;
  }

  h2{
    margin-bottom: 10px;
    color: var(--text);
  }
  </style>
  