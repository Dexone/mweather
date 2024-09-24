<template>
    <div class="flex items-center justify-between mb-4 ">    
      <h5 class="text-xl font-bold leading-none text-gray-900">Прогноз на 5 дней</h5>
  
  
  
      <a class="text-sm font-medium text-blue-600 hover:underline">
        <!-- {{ osnStore.weatherInfo[0].city }} -->
      </a>
    </div>
    <div class="flow-root">
  
  
  
  
  
      <ul class="divide-y divide-gray-200" >
  
  
  
  
        <!-- <li v-for="weather, index in sortInfo" class="py-3 sm:py-4">
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
        </li> -->
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
  
  // const sortInfo = ref([])
  
  
  // watch(osnStore, () => {
  //   sortInfo.value.length = 0 //обнуление массива 5 дневного прогноза
  //   sortInfo.value.push(osnStore.weatherInfo[0]) //пуш первого дня (сегодняшнего)
  //   osnStore.weatherInfo[0].date = "Сегодня" //присвоение первому объекту значения сегодня
  //   for (let i = 4; i < 40; i++) {
  //     if (osnStore.weatherInfo[i].dt_txt.slice(11).slice(0, 2).includes("09")) { //отбор одного значения в день в 9 утра
  //       sortInfo.value.push(osnStore.weatherInfo[i]) //пуш погоды на этот день в массив 5-дневного прогноза
  //   }}

  
  // })
  
  








// let week = []
// let date = [] //исп
// let count = 1
// let indexMonth = ['января', 'февраля', 'марта', 'апреля', 'мая', 'июня', 'июля', 'августа', 'сентября', 'октября', 'ноября', 'декабря']; //исп
// let indexNed = ['воскресенье', 'понедельник', 'вторник', 'среда', 'четверг', 'пятница', 'суббота']
// for (let i = 0; i < 40; i++) {
//     let d = new Date(new Date().getTime() + count * 60 * 60 * 1000); //исп
//     let Month = d.getMonth(); //исп
//     let Ned = d.getDay();
//     let Day = d.getDate(); //исп
//     week.push(indexNed[Ned])
//     date.push(Day + " " + indexMonth[Month])
//     count = count + 3 //исп
// }


const weatherInfo = ref([])
function getWeather() {
    axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=Kirov&units=metric&appid=a8bb29b1e583c33aa2fb3a2944930de7`).then((res) => {
        const weatherData = res.data.list.map((item, index) => {
            return {
                time: res.data.list[index].dt_txt.slice(11).slice(0, 2), //время выбранного дня
                pic: import.meta.env.BASE_URL + "min/" + res.data.list[index].weather[0].main + ".png", //картинка облачности
                pop: Math.round(res.data.list[index].pop * 100), //вероятность дождя
                deg: res.data.list[index].wind.deg, // направление ветра в градусах
                wind: Math.round(res.data.list[index].wind.speed), //скорость ветра м/с
                gust: Math.round(res.data.list[index].wind.gust), //порывы ветра м/с
                temp: Math.round(res.data.list[index].main.temp), //температура воздуха
                feels_like: Math.round(res.data.list[index].main.feels_like), //ощущается как(температура)
                humidity: res.data.list[index].main.humidity, //влажность
                grnd: Math.round(res.data.list[index].main.grnd_level / 1.333), //давление


            }
        })
        // weatherInfo.value = weatherData;


    // weatherInfo.value.push(osnStore.weatherInfo[0]) //пуш первого дня (сегодняшнего)
    // osnStore.weatherInfo[0].date = "Сегодня" //присвоение первому объекту значения сегодня
console.log(weatherData)
    weatherInfo.value.push(weatherData[0])
    let b = detailsIndex.value //индекс выбранного дня
    for (let i = 4; i < 40; i++) {
      if (weatherData[i].time.includes("09")) { //отбор одного значения в день в 9 утра
        weatherInfo.value.push(weatherData[i]) //пуш погоды на этот день в массив 5-дневного прогноза
    }}



        // for (let i = 0; i < 4; i++) { //получение 4 значений погоды (утро день...)
        //     weatherInfo.value[i] = weatherData[b]
        //     b = b + 2 //пропуск 2 значений ибо 9:00 12:00
        // }


    })
    console.log(weatherInfo)
}
getWeather()














  
  
  function scroll() {
    window.scrollTo({
      top: 530,
      behavior: 'smooth'
    })
  }
  
  
  </script>