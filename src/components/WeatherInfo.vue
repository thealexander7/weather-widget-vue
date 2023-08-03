<script setup>
import { defineProps } from 'vue';
import { IMG_BASE_URL } from '@/constants'

defineProps({
  weatherInfo: {
    type: Array
  }
})

</script>
<template>
  <div class="weather-info">
    <template v-if="weatherInfo.length < 1">
      Empty list
    </template>
    <div v-for="location in weatherInfo" :key='location.sys.id' class="weather-info__card">
      <div class="weather-info__title">{{ location.name }}, {{ location.sys?.country }}</div>
      <div class="weather-info__icon-text">
        <img class='weather-info__image' :src="`${IMG_BASE_URL}${location.weather[0].icon}.png`" alt="">
        <span class="weather-info__temperature">{{ Math.round(location.main.temp) }} °C</span>
      </div>
      <div class="weather-info__full-info">
        <p>
          Feels like {{ Math.round(location.main.feels_like) }} °C. {{ location.weather[0].main }}.
          {{ location.weather[0].description }}
        </p>

        <ul class="weather-info__list">
          <li class="weather-info__list-item">
            <i class="fa-solid fa-wind"></i>
            <span>Wind: {{ location.wind.speed }} m/s</span>
          </li>
          <li class="weather-info__list-item">
            <i class="fa-solid fa-gauge"></i>
            <span>Pressure: {{ location.main.pressure }} hPa</span>
          </li>
          <li class="weather-info__list-item">
            <i class="fa-solid fa-droplet"></i>
            <span>Humidity: {{ location.main.humidity }} %</span>
          </li>
          <li class="weather-info__list-item">
            <i class="fa-solid fa-eye-slash"></i>
            <span>Visibility: {{ (location.visibility / 1000).toFixed(1) }} km.</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style lang='scss' scoped>
@import '../assets/styles/main';
.weather-info {
  width: 270px;
  padding: 10px;

  &__card {
    border: 1px solid $main-bg-color;
    box-shadow: 0 1px 4px $main-bg-color;
    padding: 10px;
    margin-bottom: 10px;
  }

  &__title {
    font-weight: 600;
  }

  &__icon-text {
    @include flex-align-center;
    gap: 10px;
  }

  &__temperature {
    font-size: 24px;
    font-weight: 600;
  }

  &__list {
    display: flex;
    flex-direction: column;
    gap: 7px;
  }

  &__list-item {
    @include flex-align-center;
    gap: 10px;
  }
}
</style>