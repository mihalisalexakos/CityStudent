<template>
    <div class="attractions-card" v-if="places.length">
      <h2>Attractions</h2>
      <ul>
        <li v-for="place in places" :key="place.place_id">
            <strong>{{ place.name }}</strong>
            <img v-if="place.image" :src="place.image" alt="Image of attraction" />
            <div v-else class="no-image-placeholder">
                <i class="fa-solid fa-image"></i>
                no images found :/
            </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: ['places']
  }
  </script>
  
  <style scoped>

  .attractions-card {
    padding: 10px;
    background-color: var(--white);
    outline: 2px solid var(--element);
    border-radius: 5px;
    box-sizing: border-box;

    display: flex;
    flex-direction: column;

    max-height: 600px;
    overflow: auto;
  }

  ul {
    margin-top: 10px;
    list-style-type: none;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 20px;
    
  }
  li {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content:center;
    color: var(--blueDK);
  }

  li img{
    border-radius: 5px;
  }

  li .no-image-placeholder{

    height: fit-content;
    padding: 10px;
    margin-top: 10px;
    width: 95%;
    border-radius: 5px;
    background-color: var(--bg);

    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: var(--element);
  }
  li .no-image-placeholder i{
    color: var(--element);
    font-size: 50px;
  }
  </style>
  