<template>
    <div class="flex items-center justify-between mb-4 ">
        <h5 class="text-xl font-bold leading-none text-gray-900"> Подробный прогноз</h5>
        <a class="text-xl font-bold leading-none text-gray-900">
            {{ }}
        </a>
    </div>





    <ul class="my-4 space-y-3" v-for="weather, index in weatherInfo">


        <p class="text-sm font-normal text-gray-500">
            <a v-if="weather.chislo === '12' || weather.chislo === '15'">Днем</a>
            <a v-if="weather.chislo === '18' || weather.chislo === '21'">Вечером</a>
            <a v-if="weather.chislo === '00' || weather.chislo === '03'">Ночью</a>
            <a v-if="weather.chislo === '06' || weather.chislo === '09'">Утром</a>
        </p>




        <li>
            <a
                class="flex items-center p-3 text-base font-bold text-gray-900 rounded-lg bg-gray-50 hover:bg-gray-100 group hover:shadow">
                <img class="rounded-full w-6 h-6" v-bind:src="weather.pic">

                <span class="flex-1 ms-3 whitespace-nowrap">Облачно</span>
                <span
                    class="inline-flex items-center justify-center px-2 py-0.5 ms-3 text-xs font-medium text-gray-500 bg-gray-200 rounded">☔{{
                        weather.pop }}%</span>
            </a>
        </li>




        <li>
            <a
                class="flex items-center p-3 text-base font-bold text-gray-900 rounded-lg bg-gray-50 hover:bg-gray-100 group hover:shadow ">
                <img class="rounded-full w-6 h-6" :style="{ transform: 'rotate(' + weather.deg + 'deg)' }"
                    src="../assets/arrow.png">
                <span class="flex-1 ms-3 whitespace-nowrap"> <a v-if="weather.deg > 330 || weather.deg < 30">С {{
                    weather.wind }} м/с</a>
                    <a v-if="weather.deg > 30 && weather.deg < 60">СВ {{ weather.wind }} м/с</a>
                    <a v-if="weather.deg > 60 && weather.deg < 120">В {{ weather.wind }} м/с</a>
                    <a v-if="weather.deg > 120 && weather.deg < 150">ЮВ {{ weather.wind }} м/с</a>
                    <a v-if="weather.deg > 150 && weather.deg < 210">Ю {{ weather.wind }} м/с</a>
                    <a v-if="weather.deg > 210 && weather.deg < 240">ЮЗ {{ weather.wind }} м/с</a>
                    <a v-if="weather.deg > 240 && weather.deg < 300">З {{ weather.wind }} м/с</a>
                    <a v-if="weather.deg > 300 && weather.deg < 330">СЗ {{ weather.wind }} м/с</a></span>
                <span
                    class="inline-flex items-center justify-center px-2 py-0.5 ms-3 text-xs font-medium text-gray-500 bg-gray-200 rounded">Порывы
                    до {{ weather.gust }} м/с</span>
            </a>
        </li>

        <li>
            <a
                class="flex items-center p-3 text-base font-bold text-gray-900 rounded-lg bg-gray-50 hover:bg-gray-100 group hover:shadow">
                <img class="rounded-full w-6 h-6" src="../assets/temp.png">
                <span class="flex-1 ms-3 whitespace-nowrap">
                    {{ weather.temp }}°
                </span>
                <span
                    class="inline-flex items-center justify-center px-2 py-0.5 ms-3 text-xs font-medium text-gray-500 bg-gray-200 rounded">Ощущается
                    как {{ weather.feels_like }}</span>
            </a>
        </li>

        <li>
            <a
                class="flex items-center p-3 text-base font-bold text-gray-900 rounded-lg bg-gray-50 hover:bg-gray-100 group hover:shadow ">
                <img class="rounded-full w-6 h-6" src="../assets/humidity.png">

                <span class="flex-1 ms-3 whitespace-nowrap">{{ weather.humidity }}%</span>
                <span
                    class="inline-flex items-center justify-center px-2 py-0.5 ms-3 text-xs font-medium text-gray-500 bg-gray-200 rounded">
                    {{ weather.grnd }}мм рт.ст.</span>
            </a>
        </li>
    </ul>




</template>

<script setup>

import { inject, watch, ref, onDeactivated } from 'vue'
import axios from 'axios';



defineProps({
    msg: String,
})

let detailsIndex = inject("detailsIndex")



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
                chislo: res.data.list[index].dt_txt.slice(11).slice(0, 2), //число используемого дня
                pic: import.meta.env.BASE_URL + "/min/" + res.data.list[index].weather[0].main + ".png", //картинка облачности
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

        let b = detailsIndex.value //индекс выбранного дня
        for (let i = 0; i < 4; i++) { //получение 4 значений погоды (утро день...)
            weatherInfo.value[i] = weatherData[b]
            b = b + 2 //пропуск 2 значений ибо 9:00 12:00
        }


    })
}
getWeather()


</script>
