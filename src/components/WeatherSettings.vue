<script setup>
import { ref, defineEmits } from 'vue';
import  { API_KEY, BASE_URL } from '@/constants';

import { capitalizeFirstLetter } from '@/utils'

import draggable from 'vuedraggable';

const city = ref(null);
const weatherInfo = ref([]);
const drag = ref(false)

if(localStorage.getItem('listOfLocations')) {
  weatherInfo.value = JSON.parse(localStorage.getItem('listOfLocations'))
}

const getWeather = () => { 
  if(city.value) {
    fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
      .then( response => response.json())
      .then( data => {
        if(weatherInfo.value.some(item => item.name === capitalizeFirstLetter(city.value))) {
          return city.value = null
        }
        weatherInfo.value.push(data);
        emit('getWeather', weatherInfo.value);
        localStorage.setItem('listOfLocations', JSON.stringify(weatherInfo.value))
        
        city.value = null
      })
      .catch(error => console.log(error))
  }
}

const removeAt = (id) => {
  localStorage.setItem(
    'listOfLocations',
    JSON.stringify(
      JSON
        .parse(localStorage.getItem('listOfLocations'))
        .filter((item) => item.id !== id),
    )
  )
  weatherInfo.value = weatherInfo.value.filter(item => item.id !== id)
  emit('removeAt', weatherInfo.value)
}

const emit = defineEmits(['getWeather', 'removeAt'])
</script>

<template>
  <div class="weather-settings">
    <div class="weather-settings__title">Settings</div>
    <div class="weather-settings__list-cities">
      <span v-if="weatherInfo.length < 1">Empty list</span>
        <draggable v-else
          v-model="weatherInfo" 
          group="people" 
          @start="drag=true" 
          @end="drag=false"
          handle=".handle"
          class='draggable__items'
          >
          <div v-for="element in weatherInfo" :key="element.id" class="draggable__item">
            <i class="fa-solid fa-bars handle"></i>
            {{element.name}}
            <i class="fa-solid fa-trash"  @click="removeAt(element.id)"></i>
          </div>
        </draggable>
    </div>
    <div>
      <label for="search">Add locations:</label>
      <div class="weather-settings__search-wrapper">
        <input 
          class="weather-settings__search-input" 
          name='search' 
          type="text" 
          v-model.trim="city" 
          @keyup.enter='getWeather'
          placeholder='Search a new location'
        >
        <i class="fa-solid fa-arrow-left-long" @click='getWeather'></i>
      </div>
    </div>
  </div>
</template>

<style lang='scss' scoped>
@import '../assets/styles/main';
.weather-settings {
  width: 270px;

  &__title {
    font-weight: 600;
  }

  &__list-cities {
    margin: 10px 0px;
  }

  &__search-wrapper {
    @include flex-align-center;
    gap: 10px;
  }

  &__search-input {
    padding: 10px;
    border-radius: 5px;
  }
}

.draggable {
  &__items {
    display: flex;
    flex-direction: column;
    gap: 7px;
  }
  &__item {
    display: flex;
    justify-content: space-between;
    background-color: $main-bg-color;
    padding: 10px;
  }
}

.handle {
  cursor: pointer;
}
</style>