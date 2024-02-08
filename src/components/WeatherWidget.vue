<template>
  <div class=" bg-gradient-to-br from-blue-500 via-green-500 to-yellow-500 flex items-center justify-center">
    <div class=" w-full space-y-8">
      <div class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4 flex flex-col">
        <div class="mb-4">
          <label class="block text-grey-darker text-sm font-bold mb-2" for="location">
            Location
          </label>
          <input class="shadow appearance-none border rounded w-full py-2 px-3 text-grey-darker bg-white leading-tight focus:outline-none focus:shadow-outline text-black" id="location" type="text" placeholder="Enter location" v-model="location">
        </div>
        <button class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-700" @click="fetchWeatherDataByLocation">Fetch Weather Data</button>
        <div class="grid grid-cols-5 gap-4 text-black mt-4"  >
          <div v-for="(forecast, index) in dailyForecasts" :key="index" class="bg-white rounded shadow p-4">
            <h2 class="text-xl font-bold mb-2">{{ formatDate(forecast.Date) }}</h2>
            <p class="mb-2">{{ forecast.Day.IconPhrase }}</p>
            <p class="text-lg">{{ convertTemp(forecast.Temperature.Maximum.Value) }}°C</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      dailyForecasts: [],
      location: '', // New data property to hold the location input value
    };
  },
  methods: {
    formatDate(epochDate) {
      const date = new Date(epochDate);
      return `${date.getUTCDate()} ${date.toLocaleString('default', { month: 'short' })}`;
    },
    convertTemp(tempFahrenheit) {
      return Math.round((tempFahrenheit -   32) * (5 /   9));
    },
    fetchWeatherDataByLocation() {
      if (!this.location) {
        alert('Please enter a location');
        return;
      }
      axios.get(`http://localhost:8000/api/weather/${this.location}`)
        .then(response => {
          this.dailyForecasts = response.data.DailyForecasts;
        })
        .catch(error => {
          console.error('Error fetching weather data:', error);
        });
    },
  },
};
</script>

<style scoped>
/* Add any custom styles here */
</style>