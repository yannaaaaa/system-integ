<template>
  <div
    class="d-flex flex-column mx-auto justify-center align-center"
    style="min-height: 100vh; background-color: #ccecfb; padding: 20px;"
  >
    <div v-if="currentWeather" style="width: 500px; max-width: 100%;">

      <!-- Current Weather -->
      <v-card
        class="pa-8 mb-6"
        elevation="4"
        rounded="xl"
      >
        <div class="d-flex align-center">
          <v-icon color="blue">mdi-map-marker-outline</v-icon>

          <p class="ml-2" style="color: #5f91ae; font-size: 18px;">
            {{ currentWeather.location.name }},
            {{ currentWeather.location.country }}
          </p>
        </div>

        <div class="d-flex justify-space-between align-center mt-4">
          <div>
            <p
              class="font-weight-bold"
              style="font-size: 85px; color: #1e3c4d; margin: 0;"
            >
              {{ Math.round(currentWeather.current.temp_c) }}°
            </p>

            <p style="color: #5f91ae; font-size: 17px;">
              Feels like {{ Math.round(currentWeather.current.feelslike_c) }}°C
            </p>

            <v-chip color="blue-lighten-5" style="color: #4b9ed0;">
              {{ currentWeather.current.condition.text }}
            </v-chip>
          </div>

          <img
            :src="`https:${currentWeather.current.condition.icon}`"
            style="width: 120px; height: 120px;"
          />
        </div>
      </v-card>

      <!-- Forecast -->
      <v-card
        class="pa-6"
        elevation="4"
        rounded="xl"
      >
        <p
          class="font-weight-bold"
          style="color: #5c88a5; font-size: 18px; letter-spacing: 1px;"
        >
          5-DAY FORECAST
        </p>

        <div
          v-for="day in forecastDays"
          :key="day.date"
          class="d-flex align-center justify-space-between"
          style="min-height: 50px;"
        >
          <p
            style="width: 80px; color: #163d58; font-size: 18px;"
          >
            {{ formatDay(day.date) }}
          </p>

          <img
            :src="`https:${day.day.condition.icon}`"
            style="width: 40px; height: 40px;"
          />

          <div>
            <span
              class="font-weight-bold"
              style="color: #163d58; font-size: 18px;"
            >
              {{ Math.round(day.day.maxtemp_c) }}°
            </span>

            <span
              class="ml-4"
              style="color: #85b1c9; font-size: 18px;"
            >
              {{ Math.round(day.day.mintemp_c) }}°
            </span>
          </div>
        </div>
      </v-card>
    </div>

    <div v-else class="text-center">
      <v-progress-circular
        indeterminate
        color="blue"
      />

      <p class="mt-3">Loading weather...</p>
    </div>
  </div>
</template>

<script lang="ts" setup>
// @ts-nocheck

const currentWeather = ref(null)

const forecastDays = computed(() => {
  return currentWeather.value?.forecast?.forecastday || []
})

const getWeatherData = async () => {
  try {
    const data = await $fetch(
      'http://api.weatherapi.com/v1/forecast.json?key=0efc29429d57484fb5232751262906&q=Manila&days=2&aqi=no&alerts=no'
    )

    currentWeather.value = data
    console.log(data)
  } catch (error) {
    console.error('Failed fetch weather data', error)
  }
}

const formatDay = (date: string) => {
  return new Date(date).toLocaleDateString('en-US', {
    weekday: 'short'
  })
}

onMounted(getWeatherData)
</script>