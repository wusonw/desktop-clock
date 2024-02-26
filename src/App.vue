<script setup>
import { nextTick, ref } from 'vue'
import { appWindow } from '@tauri-apps/api/window';
import { dataDir } from '@tauri-apps/api/path';

// appWindow.setIgnoreCursorEvents(true)
appWindow.setAlwaysOnTop(true)

const dateTime = ref(new Date())
const WEEKDAYS = ['日', '一', '二', '三', '四', '五', '六']
const updateTime = () => {
  nextTick(() => {
    dateTime.value = new Date()
  })
  requestAnimationFrame(updateTime)
}

const parseTime = (time) => time.toLocaleString().split(/\s+/).pop()
const parseDate = (date) => {
  const weekDay = WEEKDAYS[date.getDay()]
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  return `${year} / ${month} / ${day} 周${weekDay}`
}
updateTime()

</script>

<template>
  <div class="container" data-tauri-drag-region>
    <div class="time">{{ parseTime(dateTime) }}</div>
    <div class="date">{{ parseDate(dateTime) }}</div>
  </div>
</template>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  /* background-color: rgba(41, 20, 3, 0.218); */
  border-radius: 10px;

  .time {
    text-align: center;
    font-size: 72px;
    color: white;
    font-weight: bold;
  }

  .date {
    font-size: 16px;
    color: white;
    font-weight: bold;
    text-align: end;
    padding: 0 20px;
  }
}
</style>
