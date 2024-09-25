<template>
  <div class="flex items-center justify-between mb-4 ">
    <h5 class="text-xl font-bold leading-none text-gray-900">Прогноз на 5 дней</h5>



    <a class="text-sm font-medium text-blue-600 hover:underline">
      {{ city }}
      <!-- здесь остановился, city is not defined -->
    </a>
  </div>
  <div class="flow-root">


        <!-- loader -->
        <div role="status" class=" space-y-4 divide-y divide-gray-200 rounded animate-pulse"
            v-if="weatherInfo.length === 0" v-for="i in 6">
            <div class="flex items-center justify-between">
                <div>
                    <div class="h-2.5 bg-gray-300 rounded-full w-24 mb-2.5 mt-2.5"></div>
                    <div class="w-32 h-2 bg-gray-200 rounded-full "></div>
                </div>
                <div class="h-2.5 bg-gray-300 rounded-full w-12"></div>
            </div>

            <div class="flex items-center justify-between pt-4">
            </div>
        </div>
        <!-- loader -->


    <ul class="divide-y divide-gray-200">


      <li v-for="weather, index in weatherInfo" class="py-3 sm:py-4">
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
          <button @click="detailsIndex[0] = index, detailsIndex[1] = weather.date, scroll()"
            class="inline-flex items-center justify-center px-2 py-0.5 ms-3 text-xs font-medium text-gray-500 bg-gray-200 rounded">Подробнее</button>
        </div>
      </li>
    </ul>
  </div>

</template>

<script setup>

import { inject, ref, watch } from 'vue'
import { onStartTyping } from '@vueuse/core';
import axios from 'axios';
defineProps({
  type: Object,
  required: true
})

const detailsIndex = inject("detailsIndex") //индекс дня в общем массиве при нажатии на него для подробного прогноза
const city = ref("Неизвестно")

const weatherInfo = ref([])
function getWeather() {

  let date = [] //24 сентября, 25 сентября ...
  let week = [] //понедельник, вторник..
  function datePush() { //формирует массив date с 40 значениями в виде числа и месяца (24 сентября, 25 сентября) и в return прикрепляются к каждому дню в weatherData (тоже самое с днями недели)
    let count = 1
    let indexMonth = ['января', 'февраля', 'марта', 'апреля', 'мая', 'июня', 'июля', 'августа', 'сентября', 'октября', 'ноября', 'декабря'];
    let indexNed = ['воскресенье', 'понедельник', 'вторник', 'среда', 'четверг', 'пятница', 'суббота']
    for (let i = 0; i < 40; i++) {
      let d = new Date(new Date().getTime() + count * 60 * 60 * 1000);
      let Month = d.getMonth();
      let Ned = d.getDay();
      let Day = d.getDate();
      week.push(indexNed[Ned])
      date.push(Day + " " + indexMonth[Month])
      count = count + 3
    }
  }
  datePush()


  axios.get(`https://ru.api.openweathermap.org/data/2.5/forecast?q=Kirov&units=metric&appid=a8bb29b1e583c33aa2fb3a2944930de7`).then((res) => {
    city.value = res.data.city.name // название города
    const weatherData = res.data.list.map((item, index) => {
      return {
        temp: Math.round(res.data.list[index].main.temp), //температура воздуха
        pic: import.meta.env.BASE_URL + "min/" + res.data.list[index].weather[0].main + ".png", //картинка облачности
        time: res.data.list[index].dt_txt.slice(11).slice(0, 2), //время выбранного дня
        date: date[index], //24 сентября, 25 сентября ...
        week: week[index], //понедельник, вторник..
      }

    })


    weatherInfo.value.push(weatherData[0]) //создание первого дня
    weatherInfo.value[0].date = "Сегодня" //заменя числа и месяца в первом дне на "сегодня"

    for (let i = 4; i < 40; i++) {
      if (weatherData[i].time.includes("09")) { //отбор одного значения в день в 9 утра
        weatherInfo.value.push(weatherData[i]) //пуш погоды на этот день в массив 5-дневного прогноза
      }
    }



  })
}
getWeather()
















function scroll() {
  window.scrollTo({
    top: 530,
    behavior: 'smooth'
  })
}


</script>