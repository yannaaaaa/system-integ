<template>
  <div>
        <h1>Weather</h1>
        <h2>{{ currentWeather?.current?.temp_c }}</h2>

    </div>
</template>

<script lang="ts" setup>
//@ts-nocheck
//definePageMeta({
  //layout:'weather'
//})

const currentWeather = ref(null);

  const getWeatherData2 = async () => {
  try {
    const data = await $fetch('http://api.weatherapi.com/v1/forecast.json?key=0efc29429d57484fb5232751262906&q=Manila&days=2&aqi=no&alerts=no')
    
    if (data) {
      console.log(data)
      currentWeather.value = data
    }
  } catch (error) {
    console.error('failed fetch weather data', error)
  }
}
  
onMounted(getWeatherData2);
</script>

<style>

</style>