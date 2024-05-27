<script setup lang="ts">
import { ForecastDayHour } from '../interfaces/TheWeather'

defineProps<ForecastDayHour>()

function formatTime(date: string) {
  const dateObject = new Date(date)
  const hours = dateObject.getHours()
  const minutes = dateObject.getMinutes()

  return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}`
}

function formatDate(input: string) {
  const date = new Date(input)
  const day = date.getDate().toString().padStart(2, '0')
  const month = (date.getMonth() + 1).toString().padStart(2, '0')
  const year = date.getFullYear()

  return `${day}/${month}/${year}`
}

function getCurrentUnixTimestamp() {
  return Math.floor(Date.now() / 1000)
}
</script>
<template>
  <div class="mx-auto mt-10 flex w-full gap-3 overflow-auto">
    <div
      class="flex flex-col rounded-2xl bg-white p-3 pt-3 text-center"
      v-for="(item, index) in data.filter((item) => item.time_epoch > getCurrentUnixTimestamp())"
      :key="index"
    >
      <div class="text-sm text-gray-500 dark:text-gray-400">{{ formatDate(item.time) }}</div>
      <div class="text-base text-gray-500 dark:text-gray-400">{{ formatTime(item.time) }}</div>
      <img :src="item.condition.icon" class="mx-auto" />
      <div class="mt-1.5 font-semibold text-gray-800 dark:text-gray-300">
        {{ item.temp_c }}&deg;C
      </div>
      <div class="text-sm text-gray-600 dark:text-gray-400">{{ item.condition.text }}</div>
    </div>
  </div>
</template>
