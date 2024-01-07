<template>
  <div>
    <h1>Progress Bar App</h1>
    
    <div>From:</div>
    <div class="datetime-input-box">
      <input id="from_year" type="number" v-model.number="fromYear" />
      <input id="from_month" type="number" v-model.number="fromMonth" />
      <input id="from_day" type="number" v-model.number="fromDay" />
    </div>

    <div>To:</div>
    <div class="datetime-input-box">
      <input id="to_year" type="number" v-model.number="toYear" />
      <input id="to_month" type="number" v-model.number="toMonth" />
      <input id="to_day" type="number" v-model.number="toDay" />
    </div>

    <div>
      <progress :value="progress" max="100">{{ progress }}%</progress>
      <span>{{ progress }}% ({{ daysLeft }} days left)</span>
    </div>
  </div>
</template>

<script lang="ts" setup>
const now = new Date()

const fromYear = useCookie('from_year', {
  default() {
    return now.getFullYear()
  },
})
const fromMonth = useCookie('from_month', {
  default() {
    return now.getMonth() + 1
  },
})
const fromDay = useCookie('from_day', {
  default() {
    return now.getDate()
  },
})
const toYear = useCookie('to_year', {
  default() {
    return now.getFullYear() + 1
  },
})
const toMonth = useCookie('to_month', {
  default() {
    return now.getMonth() + 1
  },
})
const toDay = useCookie('to_day', {
  default() {
    return now.getDate()
  },
})

const from = computed(() => new Date(fromYear.value, fromMonth.value - 1, fromDay.value))
const to = computed(() => new Date(toYear.value, toMonth.value - 1, toDay.value))
const diff = computed(() => to.value.getTime() - from.value.getTime())
const progress = computed(() => {
  const progress = now.getTime() - from.value.getTime()
  return Math.floor((progress / diff.value) * 100)
})
const daysLeft = computed(() => {
  return Math.floor((to.value.getTime() - now.getTime()) / 1000 / 60 / 60 / 24)
})
</script>

<style scoped>
.datetime-input-box {
  max-width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: left;

  > * {
    max-width: 60px;
  }
}
</style>
