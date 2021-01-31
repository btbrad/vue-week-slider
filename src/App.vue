<template>
  <div id="app">
    <week-slider></week-slider>
  </div>
</template>

<script>
import WeekSlider from './components/WeekSlider'

export default {
  name: 'App',
  components: {
    WeekSlider
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body {
  margin: 0;
  padding: 0;
}
* {
  box-sizing: border-box;
}
</style>
