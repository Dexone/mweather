<template>
  <div class="flex items-center justify-between mb-4 ">
    <h5 class="text-xl font-bold leading-none text-gray-900">Прогноз на 5 дней</h5>
    <a class="text-sm font-medium text-blue-600 hover:underline">
      {{ weatherInfo[0].city }}
    </a>
  </div>
  <div class="flow-root">
    <ul class="divide-y divide-gray-200">




      <li v-for="weather, index in sortInfo" class="py-3 sm:py-4">
        <div class="flex items-center">
          <div class="flex-shrink-0">
            <img class="w-8 h-8 rounded-full" v-bind:src="weather.pic">
          </div>
          <div class="flex-1 min-w-0 ms-4">
            <p class="text-sm font-medium text-gray-900 truncate">
              {{ weather.date }}
            </p>
            <p class="text-sm text-gray-500 truncate">
              {{ weather.week }}
            </p>
          </div>
          <div class="inline-flex items-center text-base font-semibold text-gray-900">
            {{ weather.temp }}°C
          </div>
          <button @click="detailsIndex = weather.number, scroll()"
            class="inline-flex items-center justify-center px-2 py-0.5 ms-3 text-xs font-medium text-gray-500 bg-gray-200 rounded">Подробнее</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>

import { inject, ref, watch } from 'vue'

defineProps({
  type: Object,
  required: true
})

let detailsIndex = inject("detailsIndex")

const weatherInfo = inject("weatherInfo")
const sortInfo = ref([])


watch(weatherInfo, () => {
  sortInfo.value.length = 0
  sortInfo.value.push(weatherInfo.value[0])
  weatherInfo.value[0].date = "Сегодня"
  for (let i = 4; i < 40; i++) {
    if (weatherInfo.value[i].dt_txt.slice(11).slice(0, 2).includes("09")) {
      sortInfo.value.push(weatherInfo.value[i])
    }
  }

})



function scroll() {
  window.scrollTo({
    top: 530,
    behavior: 'smooth'
  })
}


</script>