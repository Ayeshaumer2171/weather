<template>
  <div>
    <h1 class="main-heading">Weather App Project</h1>
    <div class="card">
      <div class="search">

        <input type="text" placeholder="Enter city name" v-model="cityName" />
        <button v-on:click="getData" :disabled="loading">
          {{ loading ? "Loading..." : "Search" }}
        </button>
      </div>

      <div v-if="loading" class="loading">
        <img :src="Loading" alt="loading-imge" width="100px">
        </div>
        
      <div v-if="weatherData && !error" class="weather">
        <h1 class="temp">Temp:{{ weatherData.temp }}°C</h1>
        <h2 class="humidity-span">Humidity:{{ weatherData.humidity }}%</h2>

        <h2 class="city">Clouds:{{ weatherData.clouds }}</h2>
        <p class="speed">Wind Speed: {{ weatherData.windSpeed }} km/h</p>
      </div>

      <div v-else-if="!loading && error" class="error">
        <h2>City not found</h2>
        <p>Please enter a valid city name.</p>
      </div>
    </div>
  </div>
</template>

<script>
import LoadingImg from "@/assets/loading.png";

export default {

  name: "WeatherApp",

  data() {
    return {
      apikey: "df1fc0a7782d491491bd18d71bd11bf0",
      apiurl: "https://api.openweathermap.org/data/2.5/weather?q=",
      cityName: "",
      weatherData: null,
      loading: false,
      error: false,
     Loading:LoadingImg
    };
  },
  methods: {

    async ApiCall(city) {
      try {
        const response = await fetch(`${this.apiurl}${city}&appid=${this.apikey}&units=metric`);
        if (!response.ok) {
          throw new Error("City not found");
        }
        const data = await response.json();
        return {
          temp: data.main.temp,
          windSpeed: data.wind.speed,
          clouds:data.clouds.all,
          humidity:data.main.humidity,
        };
      } catch (error) {
        console.error(error.message);
        return null;
      }
    },

    async getData() {
      if (!this.cityName) {
        alert("Please enter a city name.");
        return;
      }
      this.loading = true;
      this.error = false;
      this.weatherData = null; 
      const data = await this.ApiCall(this.cityName);
      if (data) {
        this.weatherData = data;
      } else {
        this.error = true; 
      }
      this.loading = false;
    },
  },
};
</script>

<style scoped>

</style>