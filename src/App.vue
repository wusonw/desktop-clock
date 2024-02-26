<script setup>
import { nextTick, ref } from 'vue'
import { appWindow } from '@tauri-apps/api/window';
import { dataDir } from '@tauri-apps/api/path';

appWindow.setIgnoreCursorEvents(true)
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
    <div class="inner-box">
      <div class="time">{{ parseTime(dateTime) }}</div>
      <div class="date">{{ parseDate(dateTime) }}</div>
    </div>
  </div>
</template>

<style scoped>
.container {
  height: 100vh;
  padding-right: 50px;
  position: relative;
  color: rgba(255, 255, 255, 0.635);

  ::before {
    content: '';
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 20px;
  }

  .inner-box {
    filter: drop-shadow(1px 1px 2px rgb(3, 3, 3));

    .time {
      text-align: end;
      font-size: 72px;
      font-weight: bold;
    }

    .date {
      font-size: 16px;
      font-weight: bold;
      text-align: end;
    }
  }


}
</style>
