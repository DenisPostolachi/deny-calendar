<script>
import RangeDatePicker from '@/components/range-datepicker/DateRangePicker.vue'

export default {
  name: 'app',
  components: {
    RangeDatePicker
  },

  data() {
    return {
      show: true,
      dates: {}
    }
  },

  computed: {
    rangeDates() {
      if (Object.keys(this.dates).length === 2) {
        return `${new Date(this.dates.start).toISOString()}  -  ${new Date(this.dates.end).toISOString()}`
      } else {
        return ""
      }
    }
  },

  methods: {
    showPicker() {
      this.show = true
    }
  },
}
</script>

<template>
  <div id="app">
    <div class="range-date-picker-container">
      <input
          class="range-date-input"
          @click="showPicker"
          :value="rangeDates"
      />
      <range-date-picker
          class="range-date-picker"
          v-model="dates"
          v-show="show"
      />
    </div>
  </div>
</template>

<style lang="scss">
.range-date-picker-container {
  display: flex;
  flex-direction: column;
  align-items: start;
}

.range-date-input {
  width: 300px;
}


</style>
