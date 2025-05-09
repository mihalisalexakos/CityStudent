<template>
  <div class="app">
    <div class="title">
      <div class="logo">
        <img src="/src/assets/CS.png" />
        <h1><span><b>City</b></span>Student</h1>

      </div>
      
      <h3>Everything you need to know about a city, all in one place</h3>
    </div>
    <div class="search-container">
      <input v-model="city" @keyup.enter="fetchWeather" placeholder="Enter city" />
      <button @click="fetchWeather">Learn</button>
    </div>
    <p v-if="error">{{ error }}</p>

    <LoadingSpinner v-if="isLoadingWeather" />
    <WeatherCard v-else-if="weather" :weather="weather" />

    <LoadingSpinner v-if="isLoadingWiki" />
    <WikipediaCard v-else-if="wikiSummary" :summary="wikiSummary" />

    <LoadingSpinner v-if="isLoadingAttractions" />
    <AttractionsCard v-else-if="attractions.length" :places="attractions" />

    <LoadingSpinner v-if="isLoadingNews" />
    <NewsCard v-else-if="newsArticles.length" :articles="newsArticles" />
    <div class="no-content-available" v-else>Could not find any news articles regarding {{ this.city }}</div>

  </div>
</template>

<script>
import CSImage from './assets/CS.png'
import axios from 'axios'
import WeatherCard from './components/WeatherCard.vue'
import WikipediaCard from './components/WikipediaCard.vue'
import AttractionsCard from './components/AttractionsCard.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'
import NewsCard from './components/NewsCard.vue'

export default {
  components: { WeatherCard, WikipediaCard, AttractionsCard, LoadingSpinner, NewsCard },

  data() {
    return {
      city: '',
      weather: null,
      wikiSummary: null,
      attractions: [],
      newsArticles: [],
      error: '',
      isLoadingWeather: false,
      isLoadingWiki: false,
      isLoadingAttractions: false,
      isLoadingNews: false
    }
  },
  methods: {
    async fetchWeather() {
      if (!this.city) return;
      this.error = ''
      this.weather = null
      try {
        const apiKey = '75256ef3cc54f7b2fe00d85616ce72a1'
        this.isLoadingWeather = true
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}&units=metric`
        )
        this.weather = response.data
        this.isLoadingWeather = false
        this.fetchWikipediaSummary(this.weather.name)
        this.fetchAttractions(this.weather.coord.lat, this.weather.coord.lon)
        this.fetchNews(this.weather.name)
      } catch (err) {
        this.error = 'City not found or API error.'
      }
    },
    async fetchWikipediaSummary(cityName) {
      try {
        const wikiUrl = `https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(cityName)}`
        this.isLoadingWiki = true
        const response = await axios.get(wikiUrl)
        this.wikiSummary = response.data
        this.isLoadingWiki = false
      } catch (err) {
        console.error('Could not load Wikipedia summary:', err)
      }
    },
    async fetchAttractions(lat, lon) {
      try {
        const apiKey = 'b3f2749ab3084abab3ffa0396c3ced31'
        const url = `https://api.geoapify.com/v2/places?categories=tourism.sights&filter=circle:${lon},${lat},5000&limit=5&apiKey=${apiKey}`
        this.isLoadingAttractions = true
        const response = await axios.get(url)
        this.attractions = await Promise.all(response.data.features.map(async (feature) => {
          const name = feature.properties.name || feature.properties.street || 'Unnamed Place'
          const image = await this.fetchWikipediaImage(name)
          this.isLoadingAttractions = false
          return {
            place_id: feature.properties.place_id,
            name,
            categories: feature.properties.categories || [],
            image
          }
        }))
      } catch (err) {
        console.error('Failed to fetch attractions:', err)
      }
    },
    async fetchWikipediaImage(placeName) {
      try {
        const url = `https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(placeName)}`
        const response = await axios.get(url)
        return response.data.thumbnail?.source || null
      } catch (err) {
        console.warn(`No Wikipedia image for "${placeName}"`)
        return null
      }
    },
    async fetchNews(cityName) {
      try {
        this.isLoadingNews = true
        const apiKey = '137381fe1adc6ba925a63d7bfa3a5d65'
        const url = `https://gnews.io/api/v4/search?q=${encodeURIComponent(cityName)}&lang=en&token=${apiKey}`
        this.isLoadingNews = true
        const response = await axios.get(url)
        this.newsArticles = response.data.articles
        this.isLoadingNews = false
      } catch (err) {
        console.error('Failed to fetch news:', err)
      } finally {
        this.isLoadingNews = false
      }
    }

  }
}
</script>

<style scoped>
input {
  padding: 15px;
  width: 70%;
  outline: none;
  border: 2px solid transparent;
  border-radius: 5px;
  transition: 0.2s;
}

input:focus{
  border: 2px solid var(--blueLT);
  transition: 0.2s;
}
.search-container{
  display: flex;
  flex-direction: row;
  gap: 20px;
  margin-top: 20px;

  grid-column: span 2;

  justify-content: center;
}

.logo{
  display: flex;
  flex-direction: row;
  gap: 10px;
  justify-content: center;
  align-items: center;
}

.logo img{
  width: 55px;
  height: 55px;
  margin-bottom: 5px;
}

.title{
  grid-column: span 2;

}

.title h1 b{
  font-weight: bold;
}

.no-content-available{
  height: 100%;
  max-height: 600px;
  display: flex;
  color: var(--blueDK);
  border: 2px solid var(--element);
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;

}
</style>
