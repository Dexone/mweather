<template>
        <div class="text-sm text-gray-500 max-w-max mx-auto mb-6">График температуры на {{detailsIndex[1]}}</div>
    <!-- loader -->
    <div v-if="graphInfo.x.length === 0" class="  rounded animate-pulse ">
        <div class="flex items-baseline mt-4">
            <div class="w-full bg-gray-200 rounded-t-lg h-72 "></div>
            <div class="w-full h-56 ms-6 bg-gray-200 rounded-t-lg "></div>
            <div class="w-full bg-gray-200 rounded-t-lg h-72 ms-6 "></div>
            <div class="w-full h-64 ms-6 bg-gray-200 rounded-t-lg "></div>
            <div class="w-full bg-gray-200 rounded-t-lg h-80 ms-6"></div>
            <div class="w-full bg-gray-200 rounded-t-lg h-72 ms-6"></div>
            <div class="w-full bg-gray-200 rounded-t-lg h-80 ms-6"></div>
        </div>
    </div>
    <!-- loader -->

    <LineChart v-if="graphInfo.x.length != 0" :chartData="lineData" :options="options"/>
</template>

<script setup>
import axios from 'axios'
import Chart from 'chart.js/auto';
import { LineChart } from "vue-chart-3"
import { computed, inject, watch, ref, reactive } from 'vue'

defineProps({
    type: Object,
    required: true
})


const graphInfo = ref({ x: [], y: [] })
const detailsIndex = inject("detailsIndex")
const qCity = inject("qCity")


function getWeather() {
    axios.get(`https://ru.api.openweathermap.org/data/2.5/forecast?${qCity.value}&units=metric&appid=a8bb29b1e583c33aa2fb3a2944930de7`).then((res) => {
        const weatherData = res.data.list.map((item, index) => {
            return {
                dt_txt: res.data.list[index].dt_txt.slice(11, -6),
                temp: Math.round(res.data.list[index].main.temp), //температура воздуха

            }
        }
        )

        let b = detailsIndex.value[0] //индекс выбранного дня
        for (let i = 0; i < 7; i++) {
            graphInfo.value.x[i] = weatherData[b].dt_txt;
            graphInfo.value.y[i] = weatherData[b].temp
            b++
        }

    })

}
getWeather()



watch([detailsIndex.value, qCity], () => {
    getWeather()
})




const lineData = computed(() => ({
    labels: graphInfo.value.x,
    boxWidth: 0,
    datasets: [
        {
            data: graphInfo.value.y,
            label: 'График температуры на ' + detailsIndex.value[1],
            borderColor: 'rgb(28, 78, 216)', //цвет линии
            borderWidth: 4, // толщина линии
            padding: 20
        },
        
    ],
    
}));




const options = reactive({
    responsive: true,
    scales: {
        y: {
            grid: {
                color: 'rgb(255, 255, 255)',
                borderColor: 'rgb(255, 255, 255)',
                
            },
            ticks: {
                backdropColor: 'rgb(255, 255, 255)',
            }
        },
        x: {
            grid: {
                display: false,
                borderColor: 'rgb(255, 255, 255)',
            },
            ticks: {

            }
        }
    },
    plugins: {
        legend: {
            display: false,
        },

    },

});

</script>