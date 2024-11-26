<template>
    <div v-if="weather?.weather" class="summary">
    <div
        class="pic-main"
    >
    <img
      :src="getWeatherImage(weather?.weather[0]?.description)"
      alt="Weather Icon"
      style="max-width: 100%;"
    />
    </div>
    <div class="weather">
        <div class="temp">
        {{ Math.round( weather?.main?.temp) }} °C
        </div>
        <div class="weather-desc text-block">
        {{ CapitalizeFirstLetter(weather?.weather[0]?.description) }}
        </div>
    </div>
    <div class="city text-block">
        {{ weather?.name }},    
        {{ weather?.sys?.country }}
    </div>
    <div class="date text-block">
        {{ newDate }}
    </div>
    </div>
</template>

<script setup>
import { CapitalizeFirstLetter } from '@/utils/firstUpperLatter';

defineProps({
  weather: {
    type: Object,
    required: true,
  },
});

const newDate = new Date().toLocaleString('en-US', {
  weekday: 'short',
  month: 'short',
  year: 'numeric',
  day: 'numeric',
});

// Функция для возвращения пробелов
function getWeatherImage(description) {
  if (!description) return '/path/to/default-image.png'; // Путь к изображению по умолчанию

  // Оставляем пробелы как есть
  const formattedDescription = description; // Не кодируем пробелы
  try {
    return require(`@/assets/img/weather-main/${formattedDescription}.png`);
  } catch (error) {
    console.error(`Image not found for description: ${formattedDescription}`, error);
    return '/path/to/default-image.png'; // Путь к изображению по умолчанию
  }
}
</script>






<style lang="scss" scoped>
@use '../assets/styles/main';

.pic-main {
  width: 60px;
  height: 60px;
  margin: 20px 0 12px;
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: contain;
}

.city {
  font-size: 24px;
}

.weather {
  margin: 0 0 20px;
  padding: 20px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.4);
}

.temp {
  padding-bottom: 8px;
  font-size: 32px;
}

.text-block {
  position: relative;
  padding-left: 24px;
  padding-bottom: 8px;
  font-size: 14px;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 20px;
    height: 20px;
    margin-right: 6px;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: contain;
  }
}

.weather-desc {
  &::before {
    background-image: url('/src/assets/img/weather.svg');
  }
}

.city {
  &::before {
    background-image: url('/src/assets/img/location.svg');
  }
}

.date {
  &::before {
    left: 2px;
    width: 15px;
    height: 15px;
    background-image: url('/src/assets/img/calendar.svg');
  }
}
</style>