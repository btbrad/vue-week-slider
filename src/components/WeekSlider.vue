<template>
  <div
    class="week-slider"
    ref="slider"
    :style="{'transform': `translateX(${-100 * activeIndex}%)`}"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd"
    @transitionend="handleTransitionEnd"
  >
    <div class="slider-item" v-for="(arr, index) in dateArr" :key="index">
      <div :class="['day-box', {'today': today.isSame(day.date, 'day')}]" v-for="(day, idx) in arr" :key="idx">
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
      dateArr: [], // 当前的日期数据
      startPos: {
        x: 0,
        y: 0
      }, // 初始pos
      activeIndex: 1,
      range: 0.3,
      screenWidth: 0,
      direction: '', // 滑动方向
      isSwitch: false // 需要滑动页面
    }
  },
  created () {
    dayjs.locale('zh-cn')
    this.getToday()
    this.$nextTick(() => {
      this.screenWidth = this.$refs.slider.getBoundingClientRect().width
    })
  },
  methods: {
    getToday () {
      this.today = dayjs()
      this.generateData()
    },
    generateData () {
      this.dateArr = [
        this.generateAWeekData(this.today.startOf('week').subtract(7, 'day').format('YYYY-MM-DD')),
        this.generateAWeekData(this.today.startOf('week').format('YYYY-MM-DD')),
        this.generateAWeekData(this.today.startOf('week').add(7, 'day').format('YYYY-MM-DD'))
      ]
    },
    generateAWeekData (start) {
      const arr = []
      for (let i = 0; i < 7; i++) {
        const date = dayjs(start).add(i, 'day')
        arr.push({
          date: date,
          weekStr: date.format('dd'),
          dateNo: date.format('DD')
        })
      }
      return arr
    },
    handleTouchStart (e) {
      this.$refs.slider.style.transition = 'none'
      this.startPos = {
        x: e.changedTouches[0].clientX,
        y: e.changedTouches[0].clientY
      }
    },
    handleTouchMove (e) {
      const { clientX: x } = e.changedTouches[0]
      const distance = x - this.startPos.x
      this.$refs.slider.style.transform = `translateX(${-(this.screenWidth * this.activeIndex) + distance}px)`
      e.preventDefault()
    },
    handleTouchEnd (e) {
      const { clientX: x } = e.changedTouches[0]
      const distance = x - this.startPos.x
      this.isSwitch = Math.abs(distance) > this.range * this.screenWidth
      this.$refs.slider.style.transition = 'all 1s'
      if (this.isSwitch) {
        if (distance > 0) {
          console.log('右')
          this.direction = 'right'
          this.activeIndex--
        } else {
          console.log('左')
          this.direction = 'left'
          this.activeIndex++
        }
      } else {
        this.activeIndex = 1
        this.$refs.slider.style.transform = `translateX(${-(this.screenWidth * this.activeIndex)}px)`
      }
    },
    handleTransitionEnd () {
      /**
       * 停止过渡动画后再处理数据，不然会再次触发动画
       */
      this.$refs.slider.style.transition = 'none'
      if (!this.isSwitch) {
        return
      }
      if (this.direction === 'right') {
        this.dateArr.pop()
        this.dateArr.unshift(this.generateAWeekData(this.dateArr[0][0].date.subtract(7, 'day').format('YYYY-MM-DD')))
        if (this.activeIndex === 0) {
          this.activeIndex = 1
        }
      }
      if (this.direction === 'left') {
        this.dateArr.shift()
        this.dateArr.push(this.generateAWeekData(this.dateArr[1][0].date.add(7, 'day').format('YYYY-MM-DD')))
        if (this.activeIndex === 2) {
          this.activeIndex = 1
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.week-slider {
  width: 100%;
  height: 80px;
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
  &:nth-of-type(1) {
    border: 1px solid #f40;
  }
  &:nth-of-type(2) {
    border: 1px solid #0f0;
  }
  &:nth-of-type(1) {
    border: 1px solid #ff0;
  }
  .day-box {
    height: 100%;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    .week-str, .day-str {
      width: 100%;
    }
    &.today {
      .day-str {
        background-color: #f40;
        border-radius: 50%;
      }
    }
  }
}
</style>
