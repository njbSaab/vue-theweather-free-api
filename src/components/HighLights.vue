<template>
    <div class="section highlights" v-if="weather?.weather">
      <div class="title">
        Today's Highlights
      </div>
      <div class="highlights-wrapper">
        <div class="highlight">
          <div class="card">
            <div class="card-title">
              Wind
            </div>
            <div class="card-pic card-pic--wind"></div>
            <div class="card-info">
              <div class="card-justify">
                <div class="info-main">
                  <div class="info-main-num">
                    {{ weather?.wind.speed ? weather?.wind.speed.toFixed(1) : 0 }}
                 </div>
                  <div class="info-main-text">
                    m/s
                  </div>
                </div> 
                <div class="info-main">
                  <div class="info-main-num">
                    {{ weather?.wind.deg || 0 }}
                  </div>
                  <div class="info-main-text">
                    deg
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="card-small">
            <div class="card-small-title">
              Wind gusts
            </div>
            <div class="card-small-info">
              <div class="card-small-data">
                <div class="info-main-num">
                 {{ weather?.wind.gust ? weather?.wind.gust.toFixed(1) : 0 }}
                </div>
                <div class="info-main-text">
                  m/s
                </div>
              </div>
              <div class="card-small-hint">
                <div class="card-small-pic card-small-pic--wind"></div>
                <div class="card-small-text text-egorova">
                  Learn
                  <a href="https://www.windy.com/articles/weather-phenomena-what-s-the-difference-between-sustained-winds-and-wind-gusts-10390?satellite,7.787,115.115,5" target="_blank">more</a>
                  about gusts
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="highlight">
          <div class="card">
            <div class="card-title">
              Pressure
            </div>
            <div class="card-pic card-pic--pressure"></div>
            <div class="card-info">
              <div class="card-centered">
                <div class="info-main">
                  <div class="info-main-num">
                    {{ weather?.main.pressure ? weather?.main.pressure.toFixed(0) : 0 }}
                  </div>
                  <div class="info-main-text">
                    mm
                  </div>
                </div> 
              </div> 
            </div>
          </div>
          <div class="card-small">
            <div class="card-small-title">
              Feels like
            </div>
            <div class="card-small-info">
              <div class="card-small-data">
                <div class="info-main-num">
                  {{ weather?.main.feels_like ? weather?.main.feels_like.toFixed(0) : 0 }}
                </div>
                <div class="info-main-text">
                  °C
                </div>
              </div>
              <div class="card-small-hint">
                <div class="card-small-pic card-small-pic--margin card-small-pic--pressure"></div>
                <div class="card-small-text">
                  How hot or cold it really feels
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="highlight">
          <div class="card">
            <div class="card-title">
              Sunrise and sunset
            </div>
            <div class="card-pic card-pic--sun"></div>
            <div class="card-info">
              <div class="states">
                <div class="state">
                  <div class="state-pic"></div>
                  <div class="state-title">
                    Sunrise
                  </div>
                  <div class="state-time">
                    {{ getSunrise(weather?.sys.sunrise) || "00:00" }}
                  </div>
                </div>
                <div class="state">
                  <div class="state-pic state-pic--flipped"></div>
                  <div class="state-title">
                    Sunset
                  </div>
                  <div class="state-time">
                    {{ getSunset(weather?.sys.sunset) || "00:00" }}
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="card-small">
            <div class="card-small-title">
              Cloudiness
            </div>
            <div class="card-small-info">
              <div class="card-small-data">
                <div class="info-main-num">
                  {{ weather?.clouds.all || 0 }}
                </div>
                <div class="info-main-text">
                  %
                </div>
              </div>
              <div class="card-small-hint">
                <div class="card-small-pic card-small-pic--sun"></div>
                <div class="card-small-text">
                  The sky fraction obscured by clouds
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script setup>

defineProps({
  weather: {
    type: Object,
    required: true
  }
})
// Определяем функции прямо в <script setup>
function getSunset(sunset) {
  return sunset ? new Date(sunset * 1000).toLocaleTimeString() : '00:00';
}

function getSunrise(sunrise) {
  return sunrise ? new Date(sunrise * 1000).toLocaleTimeString() : '00:00';
}
</script>

<style  lang="scss" scoped>
@use '../assets/styles/main';
.highlights {
  padding: 28px 16px 16px;
  background: url('/src/assets/img/gradient-4.jpg') no-repeat 0% 0%;
  background-size: cover;
  border-radius: 25px;

  &-wrapper {
    display: flex;
    justify-content: space-between;

    @media (max-width: 575px) {
      flex-direction: column;
    }
  }
}

.title {
  padding-bottom: 16px;
}

.highlight {
  width: 32%;

  @media (max-width: 575px) {
    width: 100%;
    margin-bottom: 16px;
  }
}

.card {
  min-height: 230px;
  padding: 16px;
  background: url('/src/assets/img/gradient-2.jpg') no-repeat 50% 50%;
  background-size: cover;
  border-radius: 8px;

  @media (max-width: 1199px) {
    padding: 12px;
  }

  &-centered {
    display: flex;
    justify-content: center;
    margin-top: 40px;
  }

  &-justify {
    display: flex;
    justify-content: space-between;
    margin-top: 40px;
  }

  &-title {
    padding-bottom: 12px;
    font-size: 13px;

    @media (max-width: 1199px) {
      font-size: 12px;
    }
  }

  &-pic {
    width: 100%;
    height: 90px;
    margin-bottom: 16px;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: contain;

    &--wind {
      background-image: url('/src/assets/img/equalizer (2).png');
    }

    &--pressure {
      background-image: url('/src/assets/img/barometer.png');
    }

    &--sun {
      background-image: url('/src/assets/img/sun-moving.png');
    }
  }
}

.states {
  display: flex;
  justify-content: space-between;

  &--margin {
    margin-top: 40px;
  }
}

.state {
  width: 40%;

  &:last-child {
    text-align: right;
  }

  &-pic {
    width: 20px;
    height: 20px;
    margin-bottom: 6px;
    background: url('/src/assets/img/sun.svg') no-repeat 50% 50%;
    background-size: cover;

    &--flipped {
      margin-left: auto;
      -webkit-transform: scaleX(-1);
      transform: scaleX(-1);
    }
  }

  &-title {
    font-size: 12px;
    color: #CBB26A;
  }

  &-time {
    font-size: 13px;
    font-weight: 700;

    @media (max-width: 1199px) {
      font-size: 11px;
    }
  }
}

.info-main {
  display: flex;
  align-items: flex-end;

  &:last-child {
    text-align: right;
  }

  &-num {
    font-size: 20px;

    @media (max-width: 1199px) {
      font-size: 18px;
    }
  }

  &-text {
    padding-left: 2px;
    padding-bottom: 3px;
    font-size: 13px;
    color: rgba(255, 255, 255, 0.75);

    @media (max-width: 1199px) {
      padding-bottom: 1.5px;
      font-size: 12px;
    }
  }
}

.card-small {
  margin-top: 12px;
  padding: 12px 16px;
  background: url('/src/assets/img/gradient-2.jpg') no-repeat 50% 50%;
  background-size: cover;
  border-radius: 8px;

  &-title {
    font-size: 13px;
  }

  &-info {
    display: flex;
    justify-content: space-between;
    align-items: center;

    @media (max-width: 1199px) {
      flex-direction: column;
      align-items: flex-start;
    }
  }

  &-pic {
    width: 20px;
    height: 20px;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: contain;

    @media (max-width: 1199px) {
      display: none;
    }

    &--margin {
      width: 16px;
      height: 16px;
      margin-bottom: 3px;
    }

    &--wind {
      background-image: url('/src/assets/img/gusts.svg');
    }

    &--pressure {
      background-image: url('/src/assets/img/humidity.svg');
    }

    &--sun {
      background-image: url('/src/assets/img/cloud.svg');
    }
  }

  &-data {
    display: flex;
    align-items: flex-end;
    width: 45%;

    @media (max-width: 1199px) {
      width: 100%;
      padding-top: 8px;
    }
  }

  &-hint {
    width: 55%;

    @media (max-width: 1199px) {
      width: 100%;
    }
  }

  &-text {
    font-size: 11px;
    line-height: 1.2;
    color: rgba(255, 255, 255, 0.6);

    @media (max-width: 1199px) {
      min-height: 22px;
      font-size: 9px;
    }
  }
}

</style>