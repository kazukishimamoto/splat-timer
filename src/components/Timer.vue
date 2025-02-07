<template>
  <div class="timer-container">
    <div class="circle">
      <div class="time">{{ minutes }}:{{ seconds }}</div>
      <div class="water">
        <div class="wave" :style="{ top: `${100 - progress}%` }"></div>
        <div
          class="wave"
          :style="{ top: `${100 - progress}%`, animationDelay: '-1s' }"
        ></div>
      </div>
    </div>
    <div class="row">
      <button @click="toggle">{{ isActive ? 'Pause' : '試合開始' }}</button>
      <button @click="reset">Reset</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed, watch } from 'vue'

const totalSeconds = 180
const remainingSeconds = ref(totalSeconds)
const isActive = ref(false)
let interval: number | undefined
let halfTimeAnnounced = false
let oneMinuteAnnounced = false
let thirtyFiveSecondsAnnounced = false

const minutes = computed(() =>
  String(Math.floor(remainingSeconds.value / 60)).padStart(2, '0'),
)
const seconds = computed(() =>
  String(remainingSeconds.value % 60).padStart(2, '0'),
)
const progress = computed(
  () => (1 - remainingSeconds.value / totalSeconds) * 100,
)

const toggle = () => {
  isActive.value = !isActive.value
}

const reset = () => {
  remainingSeconds.value = totalSeconds
  isActive.value = false
  halfTimeAnnounced = false
  oneMinuteAnnounced = false
  thirtyFiveSecondsAnnounced = false
}

const announce = (message: string) => {
  const utterance = new SpeechSynthesisUtterance(message)
  speechSynthesis.speak(utterance)
}

watch(remainingSeconds, (newVal) => {
  if (newVal <= totalSeconds / 2 && !halfTimeAnnounced) {
    announce('残り半分です')
    halfTimeAnnounced = true
  }
  if (newVal <= 60 && !oneMinuteAnnounced) {
    announce('残り1分です')
    oneMinuteAnnounced = true
  }
  if (newVal <= 35 && !thirtyFiveSecondsAnnounced) {
    announce('残り35秒です')
    thirtyFiveSecondsAnnounced = true
  }
})

onMounted(() => {
  interval = window.setInterval(() => {
    if (isActive.value && remainingSeconds.value > 0) {
      remainingSeconds.value -= 1
    }
  }, 1000)
})

onUnmounted(() => {
  clearInterval(interval)
})
</script>

<style scoped>
.timer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  max-width: 100%;
  padding: 0 1rem;
  box-sizing: border-box;
}

.circle {
  position: relative;
  width: 100%;
  max-width: 200px;
  height: 200px;
  border-radius: 50%;
  border: 10px solid #673ab7; /* 青紫 */
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.time {
  position: absolute;
  font-size: 3rem;
  color: #ffffff; /* 白色 */
  z-index: 1;
}

.water {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.wave {
  position: absolute;
  bottom: 0;
  width: 200%;
  height: 200%;
  background-color: #ffeb3b; /* 黄色 */
  border-radius: 40%;
  animation: wave 4s infinite linear;
}

.wave:nth-child(2) {
  opacity: 0.5;
}

@keyframes wave {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

.row {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  color: #fff;
  background-color: #673ab7; /* 青紫 */
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #5e35b1;
}
</style>
