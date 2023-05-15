<template>
  <div class="">
    <vWeather></vWeather>
  </div>
</template>

<script>
import vWeather from './components/v-weather.vue';
export default {
  name: 'App',
  components: {
    vWeather
  }
}
</script>

<style>
#app {
  /*font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;*/
}
</style>
