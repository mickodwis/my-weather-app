<template>
    <div class="weather-widget">
      <div class="location">
        <input type="text" v-model="city" placeholder="Masukkan nama kota">
        <button @click="getWeather">Dapatkan Cuaca</button>
      </div>
      <div class="weather-info" v-if="weatherData">
        <h2>{{ weatherData.name }}, {{ weatherData.sys.country }}</h2>
        <img :src="weatherIconUrl" alt="Icon cuaca">
        <p>Suhu: {{ weatherData.main.temp }}°C</p>
        <p>Cuaca: {{ weatherData.weather[0].main }}</p>
      </div>
      <div class="loading" v-else>
        Memuat data cuaca...
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        city: '',
        weatherData: null,
        weatherIconUrl: '',
      };
    },
    methods: {
      async getWeather() {
        try {
          const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=YOUR_API_KEY&units=metric`);
          this.weatherData = response.data;
          this.weatherIconUrl = `http://openweathermap.org/img/wn/${this.weatherData.weather[0].icon}@2x.png`;
        } catch (error) {
          console.error('Error fetching weather data:', error);
          this.weatherData = null;
        }
      },
    },
  };
  </script>
  
  <style>
  .weather-widget {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    background-color: #f9f9f9;
  }
  
  .location {
    margin-bottom: 20px;
  }
  
  .location input[type="text"] {
    width: calc(100% - 80px);
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px 0 0 5px;
    outline: none;
  }
  
  .location button {
    width: 80px;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 0 5px 5px 0;
    background-color: #4caf50;
    color: #fff;
    cursor: pointer;
    outline: none;
  }
  
  .weather-info {
    text-align: center;
  }
  
  .weather-info h2 {
    margin-bottom: 10px;
  }
  
  .weather-info img {
    width: 80px;
    height: 80px;
    margin-bottom: 10px;
  }
  
  .loading {
    text-align: center;
    color: #888;
  }
  </style>
  