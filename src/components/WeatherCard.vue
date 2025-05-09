<template>
    <div class="card">
      <h2>Weather</h2>
      <h3>{{ weather.weather[0].description }}</h3>
      <img :src="iconUrl" alt="weather icon" />

      <div style="margin-top: 10px;">
        <p style="color: var(--blueDK); font-weight: 600;">Temperature: {{ weather.main.temp }}°C</p>
        <p style="color: var(--blueDK);">Feels like: {{ weather.main.feels_like }}°C</p>
      </div>

      <div class="container">
            <div class="sub-container">
                <p>Pressure: {{ weather.main.pressure }}mbar</p>
            </div>
            <div class="sub-container">
                <p>Humidity: {{ weather.main.humidity }}%</p>
            </div>
            <div class="sub-container">
                <p>Wind speed: {{ weather.wind.speed }}KM/h</p>
            </div>
            <div class="sub-container">
                <p>Deg: {{ weather.wind.deg }}°</p>
            </div>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['weather'],
    computed: {
      iconUrl() {
        const icon = this.weather.weather[0].icon
        return `https://openweathermap.org/img/wn/${icon}@2x.png`
      }
    }
  }
  </script>
  
  <style scoped>
  .card {
    padding: 10px;
    background-color: var(--white);
    outline: 2px solid var(--element);
    border-radius: 5px;
    box-sizing: border-box;

    display: flex;
    flex-direction: column;

    height: 100%;
  }

  .card p{
    align-self: start;
    color: var(--blueDK);
  }

  .card h3{
    color: var(--blueDK);
  }
  .card img{
    align-self: center;
  }


  img {
    width: 80px;
  }

  .container{
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-top: 20px;
    height: 100%;
    gap: 5px;

  }

  .sub-container{
    border: 1px solid var(--element);
    border-radius: 5px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-content: center;
    height: fit-content;
    
  }
  </style>
  