<template>
  <div class="week-slider" ref="slider" @touchstart="handleTouchStart">
    <div class="slider-item" v-for="(arr, index) in dateArr" :key="index">
      <div class="day-box" v-for="(day, idx) in arr" :key="idx">
        <div class="week-str">{{ day.weekStr }}</div>
        <div class="day-str">{{ day.dateNo }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import * as dayjs from 'dayjs'
import 'dayjs/locale/zh-cn'
const updateLocale = require('dayjs/plugin/updateLocale')
dayjs.extend(updateLocale)

dayjs.updateLocale('zh-cn', {
  weekStart: 0 // 设置每周第一天为星期日
})

export default {
  name: 'WeekSlider',
  data () {
    return {
      today: '',
      dateArr: [] // 当前的日期数据
    }
  },
  created () {
    dayjs.locale('zh-cn')
    this.getToday()
    this.$nextTick(() => {
      this.initPosition()
    })
  },
  methods: {
    getToday () {
      this.today = dayjs()
      this.generateData()
    },
    generateData () {
      this.dateArr = [
        this.generateAWeekData(this.today.subtract(7, 'day').format('YYYY-MM-DD')),
        this.generateAWeekData(this.today.startOf('week').format('YYYY-MM-DD')),
        this.generateAWeekData(this.today.add(7, 'day').format('YYYY-MM-DD'))
      ]
    },
    generateAWeekData (start) {
      const arr = []
      for (let i = 0; i < 7; i++) {
        const date = dayjs(start).add(i, 'day')
        arr.push({
          weekStr: date.format('dd'),
          dateNo: date.format('DD')
        })
      }
      return arr
    },
    initPosition () {
      this.$refs.slider.style.transform = 'translateX(-100%)'
    },
    handleTouchStart (e) {
      console.log(e)
    }
  }
}
</script>

<style lang="scss" scoped>
.week-slider {
  width: 100%;
  height: 80px;
  border: 1px solid #f40;
  margin: 0;
  display: flex;
  position: relative;
}
.slider-item {
  flex-shrink: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  .day-box {
    height: 100%;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    .week-str, .day-str {
      width: 100%;
    }
  }
}
</style>
