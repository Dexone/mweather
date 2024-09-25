<template>

    <div v-if="sun.length === 0" class="space-y-2.5 animate-pulse">
        <div class="flex items-center w-full">
            <div class="h-2.5 bg-gray-200 rounded-full w-32"></div>
            <div class="h-2.5 ms-2 bg-gray-300 rounded-full  w-24"></div>
            <div class="h-2.5 ms-2 bg-gray-300 rounded-full  w-full"></div>
        </div>
        <div class="flex items-center w-full">
            <div class="h-2.5 bg-gray-200 rounded-full  w-full"></div>
                    <div class="h-2.5 ms-2 bg-gray-300 rounded-full  w-full"></div>
            <div class="h-2.5 ms-2 bg-gray-300 rounded-full w-24"></div>
        </div>
        <div class="flex items-center w-full">
            <div class="h-2.5 bg-gray-300 rounded-full w-full"></div>
            <div class="h-2.5 ms-2 bg-gray-200 rounded-full  w-80"></div>
            <div class="h-2.5 ms-2 bg-gray-300 rounded-fullw-full"></div>
        </div>
        <div class="flex items-center w-full">
            <div class="h-2.5 ms-2 bg-gray-200 rounded-full w-full"></div>
                    <div class="h-2.5 ms-2 bg-gray-300 rounded-full  w-full"></div>
            <div class="h-2.5 ms-2 bg-gray-300 rounded-full w-24"></div>
        </div>
    </div>

    <div class="flex justify-between mb-1" v-if="sun.length != 0">
        <!-- <span class="text-base font-medium text-blue-700 ">Flowbite</span> -->
        <img width="25px" src="../assets/sun-up.png">
        <span class="text-sm text-gray-500 ">Световой день</span>
        <!-- <span class="text-sm font-medium text-blue-700 ">3232</span> -->
        <img width="25px" src="../assets/sun-down.png">
    </div>
    <div class="w-full bg-gray-200 rounded-full h-2.5 " v-if="sun.length != 0">
        <div class="bg-yellow-400 h-2.5 rounded-full" :style="{ width: sun[3] + '%' }"></div>
    </div>

    <div class="flex justify-between mb-1" v-if="sun.length != 0">

        <span class="text-sm font-medium text-yellow-700 "> {{ sun[0] }}</span>
        <span class="text-sm font-medium text-yellow-700 "> {{ sun[1] }}</span>
        <span class="text-sm font-medium text-yellow-700 "> {{ sun[2] }}</span>
    </div>

</template>

<script setup>
import { inject, watch, ref } from 'vue'
import axios from 'axios';


let sun = ref([])


function getWeather() {
    
    sun.value = []
    axios.get(`https://ru.api.openweathermap.org/data/2.5/forecast?q=Kirov&units=metric&appid=a8bb29b1e583c33aa2fb3a2944930de7`).then((res) => {
        let raznHour = Math.floor((res.data.city.sunset - res.data.city.sunrise) / 60 / 60)
        let raznMin = Math.round(((res.data.city.sunset - res.data.city.sunrise) - (raznHour * 60 * 60)) / 60)
        let date = Math.round(new Date().getTime() / 1000) //time now in seconds
        let razn1 = (res.data.city.sunset - res.data.city.sunrise) / 100 // разница между датами / 100
        let razn2 = Math.round((date - res.data.city.sunrise) / razn1)
        sun.value = [(String(new Date(res.data.city.sunrise * 1000))).slice(16, 21), raznHour + "ч " + raznMin + "мин", String(new Date(res.data.city.sunset * 1000)).slice(16, 21), razn2]
    })
}
getWeather()





defineProps({
    type: Object,
    required: true
})
</script>