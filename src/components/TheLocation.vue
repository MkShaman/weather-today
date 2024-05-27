<template>
  <div class="w-full max-w-xl">
    <div
      class="rounded-lg bg-white p-5 text-lg font-bold text-gray-800 shadow-md dark:text-gray-400"
    >
      {{ country }}, {{ region }}, {{ name }}
    </div>
  </div>
</template>
<script setup lang="ts">
import { Location } from '../interfaces/TheWeather'

defineProps<Partial<Location>>()
</script>
