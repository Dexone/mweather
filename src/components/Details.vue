<template>
    <div class="flex items-center justify-between mb-4 ">

        <h5 class="text-xl font-bold leading-none text-gray-900"> Подробный прогноз</h5>
        <a class="text-xl font-bold leading-none text-gray-900">
            {{ detailsIndex[1] }}
        </a>
    </div>



    <!-- loader -->
    <div role="status" class=" space-y-4 divide-y divide-gray-200 rounded animate-pulse" v-if="weatherInfo.length === 0"
        v-for="i in 10">
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




    <ul class="my-4 space-y-3" v-for="weather, index in weatherInfo">

        <p class="text-sm font-normal text-gray-500">
            <a v-if="weather.time === '12' || weather.time === '15'">Днем</a>
            <a v-if="weather.time === '18' || weather.time === '21'">Вечером</a>
            <a v-if="weather.time === '00' || weather.time === '03'">Ночью</a>
            <a v-if="weather.time === '06' || weather.time === '09'">Утром</a>
        </p>


        <li>
            <a
                class="flex items-center p-3 text-base font-bold text-gray-900 rounded-lg bg-gray-50 hover:bg-gray-100 group hover:shadow">
                <img class="rounded-full w-6 h-6" v-bind:src="weather.pic">

                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Clouds'">Облачно</span>
                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Clear'">Ясно</span>
                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Cloud'">Переменно</span>
                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Dust'">Пыльно</span>
                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Haze'">Туман</span>
                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Rain'">Дождь</span>
                <span class="flex-1 ms-3 whitespace-nowrap" v-if="weather.weather === 'Snow'">Снег</span>
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

const qCity = inject("qCity")

let detailsIndex = inject("detailsIndex")
watch([detailsIndex.value, qCity], () => {
    getWeather()
})





const weatherInfo = ref([])
function getWeather() {
    weatherInfo.value = []
    axios.get(`https://ru.api.openweathermap.org/data/2.5/forecast?${qCity.value}&units=metric&appid=a8bb29b1e583c33aa2fb3a2944930de7`).then((res) => {
        const weatherData = res.data.list.map((item, index) => {
            return {
                time: res.data.list[index].dt_txt.slice(11).slice(0, 2), //время используемого дня
                pic: import.meta.env.BASE_URL + "min/" + res.data.list[index].weather[0].main + ".png", //картинка облачности
                pop: Math.round(res.data.list[index].pop * 100), //вероятность дождя
                deg: res.data.list[index].wind.deg, // направление ветра в градусах
                wind: Math.round(res.data.list[index].wind.speed), //скорость ветра м/с
                gust: Math.round(res.data.list[index].wind.gust), //порывы ветра м/с
                temp: Math.round(res.data.list[index].main.temp), //температура воздуха
                feels_like: Math.round(res.data.list[index].main.feels_like), //ощущается как(температура)
                humidity: res.data.list[index].main.humidity, //влажность
                grnd: Math.round(res.data.list[index].main.grnd_level / 1.333), //давление
                weather: res.data.list[index].weather[0].main //Облачно, дождь и тд


            }
        }
        )



        let b = detailsIndex.value[0] //индекс выбранного дня
        for (let i = 0; i < 4; i++) { //получение 4 значений погоды (утро день...)
            weatherInfo.value[i] = weatherData[b]
            b = b + 2 //пропуск 2 значений ибо 9:00 12:00
        }



    })

}
getWeather()





</script>
