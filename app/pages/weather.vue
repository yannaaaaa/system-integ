<template>
  <div
    class="d-flex flex-column justify-start align-center"
    style="
      min-height: 100vh;
      width: 100vw;
      background-color: #ccecfb;
      padding: 30px 20px;
      box-sizing: border-box;
    "
  >
    <h1
      class="mb-6 font-weight-bold"
      style="color: #163d58; font-size: 55px;"
    >
      Weather
    </h1>

    <v-card
      v-if="currentWeather"
      class="pa-8 mb-6"
      width="650"
      max-width="100%"
      elevation="4"
      rounded="xl"
    >
      <div class="d-flex align-center">
        <p class="mb-0 mt-0" style="font-size: 20px;">
          {{ currentWeather?.location?.name }},
          {{ currentWeather?.location?.country }}
        </p>
      </div>

      <div class="d-flex justify-space-between align-center mt-4">
        <div>
          <h1
            class="mb-0 mt-0 font-weight-bold"
            style="color: #163d58; font-size: 50px;"
          >
            {{ currentWeather?.current?.temp_c }} °C
          </h1>

          <v-chip
            class="mt-2"
            color="blue-lighten-5"
            style="color: #4b9ed0;"
          >
            {{ currentWeather?.current?.condition?.text }}
          </v-chip>
        </div>

        <img
          :src="`https:${currentWeather?.current?.condition?.icon}`"
          :alt="currentWeather?.current?.condition?.text"
          style="width: 120px; height: 120px;"
        />
      </div>
    </v-card>

    <v-card
  v-if="currentWeather"
  class="pa-8 mb-6"
  width="650"
  max-width="100%"
  elevation="4"
  rounded="xl"
>
  <p
    class="font-weight-bold"
    style="color: #5c88a5; font-size: 25px;"
  >
    7-DAY FORECAST
  </p>

  <div
    v-for="day in currentWeather.forecast.forecastday"
    :key="day.date"
    class="d-flex align-center justify-space-between"
  >
    <p style="font-size: 20px;">
      {{ formatDay(day.date) }}
    </p>

    <img
      :src="`https:${day.day.condition.icon}`"
      style="width: 55px; height: 55px;"
    />

    <p style="font-size: 20px;">
      {{ Math.round(day.day.maxtemp_c) }}°
      /
      {{ Math.round(day.day.mintemp_c) }}°
    </p>
  </div>
</v-card>

  </div>
</template>

<script lang="ts" setup>
// @ts-nocheck

const currentWeather = ref(null)

const getWeatherData2 = async () => {
  try {
    const data = await $fetch('https://api.weatherapi.com/v1/forecast.json?key=0efc29429d57484fb5232751262906&q=Manila&days=7&aqi=no&alerts=no')

    if (data) {
      console.log(data)
      currentWeather.value = data
    }
  } catch (error) {
    console.error('Failed fetch weather data', error)
  }
}

onMounted(getWeatherData2)

const formatDay = (date: string) => {
  return new Date(date).toLocaleDateString('en-US', {
    weekday: 'short'
  })
}
</script>