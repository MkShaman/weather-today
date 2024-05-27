<script setup lang="ts">
import { ref } from 'vue'
import { ForecasttWeatherData } from './interfaces/TheWeather'
import TheForm from './components/TheForm.vue'
import TheLocation from './components/TheLocation.vue'
import TheDayCards from './components/TheDayCards.vue'

const data = ref<ForecasttWeatherData | null>(null)

function handleWeatherData(weatherData: ForecasttWeatherData | null) {
  data.value = weatherData
}
</script>

<template>
  <div
    class="flex h-full min-h-screen flex-col items-center justify-start gap-4 bg-gradient-to-b from-black to-gray-600 p-5 pt-10 sm:justify-center sm:pt-5"
  >
    <TheForm @dataToParent="handleWeatherData" />
    <TheLocation v-if="data" :name="data.location.name" :country="data.location.country" />
    <TheDayCards v-if="data" :forecastday="data.forecast.forecastday" />
  </div>
</template>
