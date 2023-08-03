<script setup>
import { ref, onMounted } from 'vue';
import  { API_KEY, BASE_URL } from './constants';

import WeatherInfo from  './components/WeatherInfo.vue';
import WeatherSettings from './components/WeatherSettings.vue';

import VButton from './components/UI/VButton.vue';
import VSpinner from '@/components/shared/VSpinner.vue';

const settings = ref(false);
const weatherInfo = ref([]);
const isLoading = ref(false)

const openSettings = () => settings.value = !settings.value

// First loading with user geo
const getGeolocation = () => {
  if(localStorage.getItem('listOfLocations') !== '[]') {
    weatherInfo.value = JSON.parse(localStorage.getItem('listOfLocations'))
  } else {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) =>
          getWeatherByLocation(position.coords.latitude, position.coords.longitude),
        (error) => {
          console.error("Error retrieving geolocation:", error);
        }
        );
    } else {
      console.error("Geolocation is not supported by this browser.");
    }
  }
};

const getWeatherByLocation = (lat, lon) => {
    isLoading.value = true;
    fetch(`${BASE_URL}?lat=${lat}&lon=${lon}&units=metric&appid=${API_KEY}`)
    .then(response => response.json())
    .then(data => {
      weatherInfo.value.push(data)
      isLoading.value = false;
    })
};

// Emits subscribes

const handleInput = (value) => {
  weatherInfo.value = value
}
const onRemoveAt = (value) => {
  weatherInfo.value = value
}

onMounted(getGeolocation)
</script>

<template>
  <div id="app">
    <v-spinner v-if="isLoading"></v-spinner>
    <div v-else class="main-layout">
      <transition name="slide-fade" mode="out-in">
        <WeatherInfo v-if="!settings" :weatherInfo='weatherInfo'/>
        <WeatherSettings v-else @getWeather="handleInput" @removeAt='onRemoveAt'/>
      </transition>
      <aside class="main-layout__right-content">
        <v-button @changeView="openSettings">
          <i class="fa-solid" :class='[settings ? "fa-xmark" : "fa-gear"]'></i>
        </v-button>
      </aside>
    </div>
  </div>
</template>

<style lang="scss">
@import './assets/styles/main';
.main-layout {
  display: flex;
  padding: 10px;

  &__right-content {
    position: relative;
  }
}
</style>
