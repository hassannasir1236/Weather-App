<template>
    <div>
      <!-- <input type="text" v-model="city" @keyup.enter="getWeather" placeholder="Enter city name" />
      <button @click="getWeather">Get Weather</button> -->
      <div class="container">
        <div class="weather__header">
          <div class="weather__search">
            <input type="text" v-model="city" @keyup.enter="getWeather" placeholder="Search for a city..." class="weather__searchform" />
            <i class="fa-solid fa-magnifying-glass"></i>
            <p class="error_message">{{ this.errorMessage }}</p>
          </div>
          <div class="weather__units">
            <span class="weather_unit_celsius" @click="getCurrentInstance('metric')">°C</span>
            <span class="weather_unit_farenheit" @click="getCurrentInstance('imperial')">°F</span>
          </div>
        </div>
        <div class="weather__body">
          <h1 class="weather__city">
            <p v-if="weatherData">{{ weatherData.name }}, {{ weatherData.sys.country }}</p>
            <p v-else></p>
          </h1>
          <div class="weather__datetime"></div>
          <div class="weather__forecast">
            <p v-if="weatherData">{{ weatherData.weather[0].main }}</p>
            <p v-else></p>
          </div>
          <div class="weather__icon">
            <div v-if="weatherData">
                <!-- Display Clear Weather Image -->
                <img 
                :src="require('@/assets/cloud.png')"
                alt="Clear Weather" 
                v-if="weatherData.weather[0].main === 'Clear'" 
                />

                <!-- Display Cloudy Weather Image -->
                <img 
               :src="require('@/assets/snow.png')"
                alt="Cloudy Weather" 
                v-if="weatherData.weather[0].main === 'Snow'" 
                />

                <!-- Display Rainy Weather Image -->
                <img 
                :src="require('@/assets/sunny.png')"
                alt="Rainy Weather" 
                v-if="weatherData.weather[0].main === 'Mist'" 
                />
                
                <img 
                :src="require('@/assets/cloudSun.png')"
                alt="Rainy Weather" 
                v-if="weatherData.weather[0].main === 'Clouds'" 
                />
                
                <img 
                :src="require('@/assets/rainy.png')"
                alt="Rainy Weather" 
                v-if="weatherData.weather[0].main === 'Rain'" 
                />
                <!-- Add More Weather Conditions as Needed -->
            </div>
          </div>
          <p class="weather__temperature"></p>
          <div class="weather__minmax">
            <p v-if="weatherData">Min: {{ weatherData.main.temp_min }}{{this.unit}}</p>
            <p v-else>Min: 0°C</p>
            <p v-if="weatherData">Max: {{ weatherData.main.temp_max }}{{this.unit}}</p>
            <p v-else>Max: 0°C</p>
          </div>
        </div>
        <div class="weather__info">
          <div class="weather__card">
            <i class="fa-solid fa-temperature-full"></i>
            <div>
              <p>Real Feel</p>
              <p class="weather__realfeel" v-if="weatherData">{{ weatherData.main.feels_like }}{{this.unit}}</p>
              <p class="weather__realfeel" v-else>0{{this.unit}}</p>
            </div>
          </div>
          <div class="weather__card">
            <i class="fa-solid fa-droplet"></i>
            <div>
              <p>Humidity</p>
              <p class="weather__humidity" v-if="weatherData">{{ weatherData.main.humidity }}%</p>
              <p class="weather__humidity" v-else>0%</p>
            </div>
          </div>
          <div class="weather__card">
            <i class="fa-solid fa-wind"></i>
            <div>
              <p>Wind</p>
              <p class="weather__wind" v-if="weatherData">{{ weatherData.main.humidity }}km/h</p>
              <p class="weather__wind" v-else>km/h</p>
            </div>
          </div>
          <div class="weather__card">
            <i class="fa-solid fa-gauge-high"></i>
            <div>
              <p>Pressure</p>
              <p class="weather__pressure" v-if="weatherData">Pressure: {{ weatherData.main.pressure }} hPa</p>
              <p class="weather__pressure" v-else>Pressure: 0 hPa</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'WeatherPage',
    data() {
      return {
        city: 'Okara',
        apiunit: 'metric',
        unit: '°C',
        weatherData: null,
        errorMessage: '',
      };
    },
    mounted() {
      this.getWeather();
    },
    methods: {
      async getWeather() {
        if (this.city === '') {
          this.errorMessage = 'Please enter a city name';
          return;
        }
        try {
          const apikey = '70305729553a52618937f81b3b7afb15';
          this.weatherData = null;
          const response = await axios.get('https://api.openweathermap.org/data/2.5/weather', {
            params: {
              q: this.city,
              appid: apikey,
              units: this.apiunit,
            },
          });
          this.errorMessage = '';
          this.weatherData = response.data;
        } catch (error) {
          this.errorMessage = 'City not found or error in fetching data.';
          this.weatherData = null;
        }
      },
      getCurrentInstance(unit){
        this.apiunit = unit;
        this.unit = unit === 'metric' ? '°C' : '°F';
        this.getWeather();
      }
    },
  };
  </script>
  
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
}
.error_message {
    color: red;
    font-size: 14px;
    margin-top: 5px;
}
.container {
    background: #171717;
    color: #fff;
    padding: 2rem;
    width: 40%;
    margin: 4rem auto;
    border-radius: 10px;
}

.weather__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

input {
    border: none;
    background: #1e1e1e;
    outline: none;
    color: #fff;
    padding: 0.5rem 2.5rem;
    border-radius: 5px;
}

input::placeholder {
    color: #fff;
}

.weather__search {
    position: relative;
}

.weather__search i {
    position: absolute;
    left: 10px;
    top: 10px;
    font-size: 15px;
    color: #fff;
}

.weather__units {
    font-size: 1.5rem;
}

.weather__units span {
    cursor: pointer;
}

.weather__units span:first-child {
    margin-right: 0.5rem;
}

.weather__body {
    text-align: center;
    margin-top: 3rem;
}

.weather__datetime {
    margin-bottom: 2rem;
    font-size: 14px;
}

.weather__forecast {
    background: #1e1e1e;
    display: inline-block;
    padding: 0.5rem 1rem;
    border-radius: 30px;
}

.weather__icon img {
    width: 100px;
}

.weather__temperature {
    font-size: 1.75rem;
}

.weather__minmax {
    display: flex;
    justify-content: center;
}

.weather__minmax p {
    font-size: 14px;
    margin: 0.5rem;
}

.weather__info {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 1rem;
    margin-top: 3rem;
}

.weather__card {
    display: flex;
    align-items: center;
    background: #1e1e1e;
    padding: 1rem;
    border-radius: 10px;
}

.weather__card i {
    font-size: 1.5rem;
    margin-right: 1rem;
}

.weather__card p {
    font-size: 14px;
}

@media(max-width: 936px){
    .container {
        width: 90%;
    }

    .weather__header {
        flex-direction: column;
    }

    .weather__units {
        margin-top: 1rem;
    }
}


@media(max-width: 400px){
    .weather__info {
        grid-template-columns: none;
    }
}
  </style>
  