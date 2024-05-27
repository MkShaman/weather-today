<template>
  <div class="flex w-full max-w-xl flex-col justify-center gap-4">
    <div class="relative w-full rounded-lg bg-white p-10 shadow-md">
      <div
        v-show="loading"
        class="absolute left-0 top-0 z-10 flex h-full w-full items-center justify-center bg-white bg-opacity-80"
      >
        <img src="../assets/loader.svg" alt="" />
      </div>
      <div class="flex items-center justify-center gap-2">
        <div class="relative w-full">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="City"
            class="w-full rounded-md border border-gray-300 p-2"
            id="cityInput"
            @keyup.enter="getForecastWeather()"
            @input="autocomplete($event)"
          />
          <ul
            v-if="autocompleteResult.length"
            class="absolute left-0 top-full flex w-full flex-col border border-black bg-white"
          >
            <li
              class="cursor-pointer border-b border-black p-2 last:border-b-0 hover:bg-slate-600 hover:text-white"
              v-for="(item, index) in autocompleteResult"
              :key="index"
              @click="getForecastWeather(item.id)"
            >
              {{ item.country }}, {{ item.name }}
            </li>
          </ul>
        </div>
        <button
          @click="getForecastWeather()"
          class="rounded-md bg-gray-900 px-4 py-2 text-white transition duration-200 hover:bg-gray-700"
        >
          Get
        </button>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import { ForecasttWeatherData, LocationData } from '../interfaces/TheWeather'

const data = ref<ForecasttWeatherData | null>(null)
const searchQuery = ref('')
const loading = ref(false)
const emit = defineEmits(['dataToParent'])
const autocompleteResult = ref<LocationData[]>([])

async function getForecastWeather(id: number = 0) {
  if (searchQuery.value) {
    try {
      loading.value = true
      let query = id ? 'id:' + id : searchQuery.value
      const response = await axios.get(
        `${import.meta.env.VITE_APP_ROOT_API}forecast.json?key=${import.meta.env.VITE_APP_ACCESS_KEY}&q=${query}&days=3`
      )
      searchQuery.value = ''
      data.value = response.data
      loading.value = false
      emitData()
      autocompleteResult.value = []
    } catch (error: any) {
      alert(error.message)
    }
  }
}

function emitData() {
  emit('dataToParent', data.value)
}

async function autocomplete(event: any) {
  if (event.target.value.length > 0) {
    try {
      const response = await axios.get(
        `${import.meta.env.VITE_APP_ROOT_API}search.json?key=${import.meta.env.VITE_APP_ACCESS_KEY}&q=${event.target.value}`
      )
      autocompleteResult.value = response.data
    } catch (error: any) {
      alert(error.message)
    }
  }
}
</script>
