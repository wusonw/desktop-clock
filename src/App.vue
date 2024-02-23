<script setup>
import { nextTick, ref } from 'vue'
import { appWindow } from '@tauri-apps/api/window';

appWindow.setIgnoreCursorEvents(true)

const time = ref(new Date())
const updateTime = () => {
  nextTick(() => {
    time.value = new Date()
  })
  requestAnimationFrame(updateTime)
}

const parseTime = (time) => time.toLocaleString().split(/\s+/).pop()
updateTime()

</script>

<template>
  <div class="container" data-tauri-drag-region>
    <span class="time">{{ parseTime(time) }}</span>
  </div>
</template>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(41, 20, 3, 0.218);
  border-radius: 10px;

  .time {
    font-size: 72px;
    color: white;
    font-weight: bold;
  }
}
</style>
