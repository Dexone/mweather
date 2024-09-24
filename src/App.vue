<!-- <script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Vite + Vue" />
  <h1 class=" font-bold underline">
    Hello world!
  </h1>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style> -->







<template>
  <!-- max-w-screen-xl -->
  <main class="bg-white rounded-lg shadow max-w-3xl mx-auto">
    <div class="w-full max-w-screen-xl mx-auto p-4 md:py-8">

      <nav class="bg-white border-gray-200 ">
        <div class="flex flex-wrap justify-between items-center mx-auto max-w-screen-xl  pb-5">
          <a href="https://dexone.github.io/mweather/" class="flex items-center space-x-3 rtl:space-x-reverse">
            <img src="../public/logo.png" class="h-8" />
            <span
              class="self-center text-2xl font-semibold whitespace-nowrap underline underline-offset-3 decoration-4 decoration-blue-400">mweather</span>
          </a>


          <div class="flex items-center space-x-6 rtl:space-x-reverse" >
            <a  class="text-sm  text-gray-500  hover:underline"><svg class="w-5 h-5 text-gray-800" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.651 7.65a7.131 7.131 0 0 0-12.68 3.15M18.001 4v4h-4m-7.652 8.35a7.13 7.13 0 0 0 12.68-3.15M6 20v-4h4"/>
</svg>
</a>
          </div>


<!-- loader -->
          <!-- <div class="flex items-center space-x-6 rtl:space-x-reverse">
            <svg aria-hidden="true" class="inline w-4 h-4 text-gray-200 animate-spin fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
        <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
    </svg>
</div> -->
<!-- loader -->
          
        </div>
        
      </nav>





      <!-- <div class="max-w-3xl bg-white divide-y divide-gray-100 rounded-lg shadow  mx-auto  mb-3">
        <Favourite />
      </div>

      <div class=" max-w-3xl p-4 bg-white border border-gray-200 rounded-lg shadow sm:p-8  mb-3 mx-auto">
        <Weather5day />
      </div> -->

      <div class="max-w-3xl p-4 bg-white border border-gray-200 rounded-lg shadow sm:p-6   mb-3 mx-auto">
        <Details />
      </div>

      <!-- <div class="max-w-3xl p-4 bg-white border border-gray-200 rounded-lg shadow sm:p-6   mb-3 mx-auto">
        <Sun />
      </div>

      <div class="max-w-3xl p-6 bg-white border border-gray-200 rounded-lg shadow mb-3 mx-auto">
        <Graph />
      </div> -->






      <hr class="my-6 border-gray-200 sm:mx-auto lg:my-8" />
      <span class="block text-sm text-gray-500 sm:text-center ">© 2024 <a href="https://github.com/Dexone"
          class="hover:underline">Dexone</a>. All Rights Reserved.</span>
    </div>



  </main>
</template>


<script setup>
// import Weather5day from "./components/Weather5day.vue"
// import Sun from "./components/Sun.vue"
// import Favourite from "./components/Favourite.vue"
// import Graph from "./components/Graph.vue"
import Details from "./components/Details.vue"
import axios from 'axios'
import { provide, ref, watch } from 'vue'





// const city = ref("q=Москва")
// provide("city", city)


// osnStore.getWeather()

// let graphInfo = ref([])
// provide("graphInfo", graphInfo)

// const inputCity = ref('')
// provide("inputCity", inputCity)

// const favouriteCity = ref(["Москва", "Санкт-Петербург", "Сочи"])
// provide("favouriteCity", favouriteCity)

let detailsIndex = ref(0) //индекс дня в общем массиве при нажатии на него для подробного прогноза
provide("detailsIndex", detailsIndex)


// watch([city, detailsIndex.value], () => {
//   getWeather()
// })





// getWeather()
</script>
