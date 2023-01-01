<script>
import date from './DateComponent.vue'

export default {
  name: "VueRangeDatePicker",

  components: {
    date
  },

  props: {
    value: {
      type: Object,
      default: null,
    },
    today: {
      type: String,
      default: "#343434"
    },
    selectedDay: {
      type: String,
      default: "#969696"
    },
    colorActive: {
      type: String,
      default: "#c7c7c7"
    }
  },

  data() {
    return {
      leftSide: this.renderLeftSide(),
      leftMonth: this.initLeftMonth(),
      rightSide: this.initRightYear(),
      rightMonth: this.initRightMonth(),
      certainDays: this.initCertainDays(),
      hoverDay: this.initHoverDay(),
    }
  },

  watch: {
    certainDays(val) {
      if (val.length === 2) {
        this.$emit('input', {
          start: Math.min(...val),
          end: Math.max(...val)
        })
      }
    }
  },

  computed: {
    selected() {
      return this.certainDays.length === 2
    },
    canHover() {
      return this.certainDays.length === 1
    },
    canPrevMonth() {
      if (this.rightSide - this.leftSide > 1) {
        return true
      } else if (this.rightSide - this.leftSide === 1) {
        return this.rightMonth - this.leftMonth > -11
      } else if (this.rightSide === this.leftSide) {
        return this.rightMonth - this.leftMonth > 1
      } else {
        return false
      }
    },
    canPrevYear() {
      if (this.rightSide - this.leftSide > 1) {
        return true
      } else if (this.rightSide - this.leftSide === 1) {
        return this.rightMonth > this.leftMonth
      } else {
        return false
      }
    },
    canNextMonth() {
      if (this.rightSide - this.leftSide > 1) {
        return true
      } else if (this.rightSide - this.leftSide === 1) {
        return this.rightMonth - this.leftMonth > -11
      } else if (this.rightSide === this.leftSide) {
        return this.rightMonth - this.leftMonth > 1
      } else {
        return false
      }
    },
    canNextYear() {
      if (this.rightSide - this.leftSide > 1) {
        return true
      } else if (this.rightSide - this.leftSide === 1) {
        return this.rightMonth > this.leftMonth
      } else {
        return false
      }
    }
  },

  methods: {
    renderLeftSide() {
      if (this.value.start) {
        return new Date(this.value.start).getFullYear()
      }
      return new Date().getFullYear()
    },
    initLeftMonth() {
      if (this.value.start) {
        return new Date(this.value.start).getMonth()
      }
      return new Date().getMonth()
    },
    initRightYear() {
      if (this.value.start && this.value.end) {
        if (this.isSameYearMonth()) {
          const leftDate = new Date(this.value.start)
          return leftDate.getMonth() === 11 ? leftDate.getFullYear() + 1 : leftDate.getFullYear()
        }
        return new Date(this.value.end).getFullYear()
      }
      return new Date().getMonth() === 11 ? new Date().getFullYear() + 1 : new Date().getFullYear()
    },
    initRightMonth() {
      if (this.value.start && this.value.end) {
        if (this.isSameYearMonth()) {
          const leftDate = new Date(this.value.start)
          return leftDate.getMonth() === 11 ? 0 : leftDate.getMonth() + 1
        }
        return new Date(this.value.end).getMonth()
      }
      return new Date().getMonth() === 11 ? 0 : new Date().getMonth() + 1
    },
    isSameYearMonth() {
      const leftSide = new Date(this.value.start).getFullYear()
      const leftMonth = new Date(this.value.start).getMonth()
      const rightSide = new Date(this.value.end).getFullYear()
      const rightMonth = new Date(this.value.end).getMonth()

      return leftSide === rightSide && leftMonth === rightMonth
    },
    initCertainDays() {
      const certainDays = []
      const {start, end} = this.value
      if (start) certainDays.push(this.formatDayToMidnight(start))
      if (end) certainDays.push(this.formatDayToMidnight(end))
      return certainDays
    },
    formatDayToMidnight(day) {
      const date = new Date(day)
      date.setHours(0)
      date.setMinutes(0)
      date.setSeconds(0)
      date.setMilliseconds(0)
      return Date.parse(String(date))
    },
    initHoverDay() {
      if (this.value.start && this.value.end && (this.value.start < this.value.end)) return this.formatDayToMidnight(this.value.end)
      return null
    },
    leftNextYear() {
      this.leftSide += 1
    },
    leftNextMonth() {
      if (this.leftMonth === 11) {
        this.leftMonth = 0
        this.leftSide += 1
      } else {
        this.leftMonth += 1
      }
    },
    leftPrevYear() {
      this.leftSide -= 1
    },
    leftPrevMonth() {
      if (this.leftMonth === 0) {
        this.leftMonth = 11
        this.leftSide -= 1
      } else {
        this.leftMonth -= 1
      }
    },
    rightNextYear() {
      this.rightSide += 1
    },
    rightNextMonth() {
      if (this.rightMonth === 11) {
        this.rightMonth = 0
        this.rightSide += 1
      } else {
        this.rightMonth += 1
      }
    },
    rightPrevYear() {
      this.rightSide -= 1
    },
    rightPrevMonth() {
      if (this.rightMonth === 0) {
        this.rightMonth = 11
        this.rightSide -= 1
      } else {
        this.rightMonth -= 1
      }
    }
  }
}
</script>

<template>
  <div class="picker">
    <div class="dates-wrapper">
      <date
          :year="leftSide"
          :month="leftMonth"
          :canNextMonth="canNextMonth"
          :canNextYear="canNextYear"
          :canHover="canHover"
          :certainDays="certainDays"
          :hoverDay="hoverDay"
          :today="today"
          :selectedDay="selectedDay"
          :colorActive="colorActive"
          @prevYear="leftPrevYear"
          @nextYear="leftNextYear"
          @prevMonth="leftPrevMonth"
          @nextMonth="leftNextMonth"
      />
      <date
          :year="rightSide"
          :month="rightMonth"
          :canPrevMonth="canPrevMonth"
          :canPrevYear="canPrevYear"
          :canHover="canHover"
          :certainDays="certainDays"
          :hoverDay="hoverDay"
          :today="today"
          :selectedDay="selectedDay"
          :colorActive="colorActive"
          @prevYear="rightPrevYear"
          @nextYear="rightNextYear"
          @prevMonth="rightPrevMonth"
          @nextMonth="rightNextMonth"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.picker {
  background-color: #fff;
  box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.1);
  width: 640px;
  margin-top: 15px;

  .dates-wrapper {
    display: flex;
    border-top: 1px solid #e6e6e6;
    position: relative;

    &::before {
      content: '';
      display: block;
      width: 1px;
      height: 100%;
      background-color: #e6e6e6;
      position: absolute;
      left: 320px;
      top: 0;
    }
  }
}
</style>

