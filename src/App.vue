<template>
  <div id="app">
    <week-slider @date-click="handleDateClick" @week-switch="handleWeekChange"></week-slider>
  </div>
</template>

<script>
import WeekSlider from './components/WeekSlider'

export default {
  name: 'App',
  components: {
    WeekSlider
  },
  methods: {
    handleDateClick (val) {
      console.log('选中日期', val)
    },
    handleWeekChange (val) {
      console.log('切换周', val)
    }
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
  overflow: hidden;
}
* {
  box-sizing: border-box;
}
</style>
