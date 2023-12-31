<script setup>
import { onMounted, ref } from "vue"
import Coords from "./components/Coords.vue"
import Highlights from "./components/Highlights.vue"
import Humidity from "./components/Humidity.vue"
import WeatherSummary from "./components/WeatherSummary.vue"
import { API_KEY, BASE_URL } from "./constants"

const city = ref("Moscow")
const weatherInfo = ref(null)

function getWeather() {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => (weatherInfo.value = data))
}

onMounted(getWeather)
</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section>
              <div class="info">
                <div class="city-inner">
                  <input @keyup.enter="getWeather" v-model="city" type="text" class="search" />
                </div>
                <WeatherSummary :weatherInfo="weatherInfo" />
              </div>
            </section>
            <section class="section section-right">
              <Highlights :weatherInfo="weatherInfo" />
            </section>
          </div>
          <div class="sections">
            <Coords :weatherInfo="weatherInfo" />
            <Humidity :weatherInfo="weatherInfo" />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="scss" scoped>
@import './assets/styles/main';

.page {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px 0;
  background-color: #59585d;
}


.laptop {
  width: 100%;
  padding: 20px;
  background-color: #0e100f;
  border-radius: 25px;
}


.sections {
  display: flex;
  width: 100%;
}


.section-left {
  width: 30%;
  padding-right: 10px;
}


.section-right {
  width: 70%;
  padding-left: 10px;
}

.city-inner {
  position: relative;
  display: inline-block;
  width: 100%;

  &::after {
    content: '';
    position: absolute;
    top: 0;
    right: 10px;
    width: 25px;
    height: 25px;
    background: url('./assets/img/search.svg') no-repeat 50% 50%;
    background-size: contain;
    transform: translateY(50%);
    cursor: pointer;
  }
}


.info {
  height: 100%;
  padding: 16px;
  background: url('./assets/img/gradient-1.jpg') no-repeat 50% 50%;
  background-size: cover;
  border-radius: 25px;
}


.search {
  width: 100%;
  padding: 16px;
  font-family: 'Inter', Arial, sans-serif;
  color: $white;
  background-color: rgba(0, 0, 0, 0.75);
  border-radius: 16px;
  border: none;
  outline: none;
  cursor: pointer;
}


.section-bottom {
  width: 50%;
  margin-top: 16px;
}
</style>
