<template>
  <audio ref="player" src="/1_Intro_MAIN.mp3"></audio>

  <div v-for="(item, index) in data" :key="index" :id="`_${index + 1}`" style="gap: 20px; margin-bottom: 20px;"
    class="grid grid-cols-[1fr_100px_100px] ">

    <span>{{ item.text }}</span>

    <button style="width: 100px;" @click="playPart(index, item.mainStartTime, item.endTime, item.startTime)">
      {{ playingIndex === index ? 'pause' : 'play' }}
    </button>

    <button style="width: 100px;" @click="rePlay(index, item.mainStartTime, item.endTime, item.startTime)">
      rePlay
    </button>

  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'


const player = ref(null)
const playingIndex = ref(null) // 1
let currentTime = ref(null)

const data = reactive([
  {
    text: 'These lessons are sold only on our website, EffortlessEnglishClub.com. If you bought these lessons somewhere else, you have an illegal copy. Please notify us and we will take immediate legal action against the seller. Thank you',
    mainStartTime: '00:00:00',
    endTime: '00:00:20',
    startTime: '00:00:00'
  },
  {
    text: 'Hi, this is AJ Hoge, Director of the Effortless English Club. Welcome to our new set of lessons, these are called the Power English Lessons.',
    mainStartTime: '00:00:20',
    endTime: '00:00:29',
    startTime: '00:00:20'
  },
  {
    text: 'The reason they’re called Power English is because in these lessons we’re going to do two things',
    mainStartTime: '00:00:29',
    endTime: '00:00:35',
    startTime: '00:00:29'
  },
  {
    text: 'Number one, you’re going to learn English, of courseAs always, we have the mini‑stories which are our favorite lessons for most of our members',
    mainStartTime: '00:00:35',
    endTime: '00:00:43',
    startTime: '00:00:35'
  },
  {
    text: 'but the content, the focus, the topics in these lessons are going to be focused on the psychology of learning and the psychology of success',
    mainStartTime: '00:00:43.050',
    endTime: '00:00:53.670',
    startTime: '00:00:43.050'
  },
  {
    text: `Now these ideas, they’re not just my ideas. In fact, they come from a lot of other people`,
    mainStartTime: '00:00:54.250',
    endTime: '00:00:58.011',
    startTime: '00:00:54.250'
  },
  {
    text: `They come from people like Tony Robbins, who is a famous success coach,
Robert Anton Wilson, Joe Vitale, Robert Kiyosaki, Alan Watt, so I’ve taken a lot of ideas
from a lot of different people about this topic of the psychology of success`,
    mainStartTime: '00:00:58.930',
    endTime: '00:01:15.071',
    startTime: '00:00:58.930'
  }
])

function timeToSeconds(timeStr) {
  const parts = timeStr.split(':') // ["hh", "mm", "ss.ms"]
  const hours = parseInt(parts[0], 10)
  const minutes = parseInt(parts[1], 10)
  const seconds = parseFloat(parts[2]) // ممكن يكون فيه decimals
  return hours * 3600 + minutes * 60 + seconds
}
function secondsToTime(seconds) {
  const h = Math.floor(seconds / 3600)
  const m = Math.floor((seconds % 3600) / 60)
  const s = (seconds % 60).toFixed(3) // 3 أرقام عشرية للمللي ثانية
  const hh = h.toString().padStart(2, '0')
  const mm = m.toString().padStart(2, '0')
  const ss = s.toString().padStart(6, '0') // لو عايز ms مع الثلاث أصفار
  return `${hh}:${mm}:${ss}`
}


function playPart(index, mainStartTime, endTime, startTime) {
  if (!player.value) return;

  const startSec = timeToSeconds(startTime)
  const endSec = timeToSeconds(endTime)
  const mainStartSec = timeToSeconds(mainStartTime)


  if (playingIndex.value === index) {
    data[index].startTime = secondsToTime(player.value.currentTime.toFixed(3))
    player.value.pause()
    playingIndex.value = null
    return
  }

  playingIndex.value = index
  player.value.currentTime = startSec

  player.value.play()

  const updateHandler = () => {
    if (player.value.currentTime >= endSec) {
      data[index].startTime = mainStartTime

      player.value.pause()
      playingIndex.value = null
      player.value.removeEventListener('timeupdate', updateHandler)
    }
  }

  player.value.addEventListener('timeupdate', updateHandler)
}

function rePlay(index, mainStartTime, endTime, startTime) {
  player.value.pause()
  player.value.currentTime = timeToSeconds(mainStartTime);
  player.value.play();
}

</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
