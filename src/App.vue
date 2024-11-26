<template>
    <div class="page">
      <main class="main">
        <div class="container">
          <div class="laptop">
            <div class="sections">
              <section class="section section-left">
                <div class="info">
                  <div class="city-inner">
                    <input v-model="city" @keyup.enter="getWeather" type="text" class="search">
                  </div>
                  <!--! WeatherSummary  -->
                  <WeatherSummary :weather="weather"/>
                </div>
              </section>
              <section class="section section-right">
                <!--! Highlight  -->
                <HighLights :weather="weather" />
              </section> 
            </div>
            <div class="sections" v-if="weather?.weather">
              <!--! Coords  -->
              <WeatherCoords :coord="weather?.coord" />              
              <!--! Humidity  -->
              <WeatherHumidity :humidity="weather?.main?.humidity"/>
            </div>
          </div>
        </div>
      </main>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'; // Импорт API из Vue
import WeatherSummary from './components/WeatherSummary.vue';
import HighLights from './components/HighLights.vue';
import WeatherCoords from './components/WeatherCoords.vue';
import WeatherHumidity from './components/WeatherHumidity.vue';
import {apiKey, baseUrl} from './constants'

const city = ref('Kyiv')
const weather = ref(null)

function getWeather() {
  try {
    fetch(`${baseUrl}?q=${city.value}&units=metric&appid=${apiKey}`)
      .then(response => {
        // Проверяем, был ли запрос успешным
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
      })
      .then(data => {
        weather.value = data;
        console.table('Weather data:', data); // Вывод данных в консоль
      })
      .catch(error => {
        // Обработка ошибок запроса
        console.error('Error fetching weather data:', error);
      });
  } catch (error) {
    // Обработка ошибок вне fetch
    console.error('Unexpected error:', error);
  }
}

onMounted(getWeather);
</script>

<style lang="scss" scoped>
@use './assets/styles/main';

.page {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px 0;
  background: linear-gradient(120deg, #39139B 0%, #0D678E 100%);
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

  @media (max-width: 767px) {
    flex-direction: column;
  }
}

.section-left {
  width: 30%;
  padding-right: 10px;

  @media (max-width: 767px) {
    width: 100%;
    padding-right: 0;
  }
}

.section-right {
  width: 70%;
  padding-left: 10px;

  @media (max-width: 767px) {
    width: 100%;
    margin-top: 16px;
    padding-left: 0;
  }
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
  color: #fff;
  background-color: rgba(0, 0, 0, 0.75);
  border-radius: 16px;
  border: none;
  outline: none;
  cursor: pointer;
}

.section-bottom {
  width: 50%;
  margin-top: 16px;

  @media (max-width: 767px) {
    width: 100%;
  }
}

</style>