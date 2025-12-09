<template>
  <audio ref="player" src="/1_Intro_MAIN.mp3"></audio>
  <div v-for="(item, index) in data" :key="index" :id="`_${index + 1}`"
    class="grid grid-cols-[1fr_100px_100px] mb-5 m-auto mt-10 gap-5 max-w-4xl">


    <span class=" ">{{ index + 1 }} => {{ item.text }}</span>

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

const data = reactive([{
  text: "These lessons are sold only on our website, EffortlessEnglishClub.com.",
  mainStartTime: "00:00:00.000",
  startTime: "00:00:00.000",
  endTime: "00:00:06.680"
},
{
  text: "If you bought these lessons somewhere else, you have an illegal copy.",
  mainStartTime: "00:00:06.680",
  startTime: "00:00:06.680",
  endTime: "00:00:11.820"
},
{
  text: "Please notify us, and we will take immediate legal action against the seller. Thank you.",
  mainStartTime: "00:00:11.820",
  startTime: "00:00:11.820",
  endTime: "00:00:19.160"
},
{
  text: "Hi, this is A.J. Hogue, director of the Effortless English Club.",
  mainStartTime: "00:00:20.110",
  startTime: "00:00:20.110",
  endTime: "00:00:24.310"
},
{
  text: "Welcome to our new set of lessons. These are called the Power English Lessons.",
  mainStartTime: "00:00:24.310",
  startTime: "00:00:24.310",
  endTime: "00:00:29.130"
},
{
  text: "The reason they're called Power English is because in these lessons, we're going to do two things.",
  mainStartTime: "00:00:29.130",
  startTime: "00:00:29.130",
  endTime: "00:00:35.171"
},
{
  text: "Number one, you're going to learn English, of course.",
  mainStartTime: "00:00:35.171",
  startTime: "00:00:35.171",
  endTime: "00:00:37.791"
},
{
  text: "As always, we have the mini-stories, which are our favorite lessons for most of our members.",
  mainStartTime: "00:00:37.791",
  startTime: "00:00:37.791",
  endTime: "00:00:43.050"
}])


function timeToSeconds(timeStr) {
  const parts = timeStr.split(':')
  const hours = parseInt(parts[0], 10)
  const minutes = parseInt(parts[1], 10)
  const seconds = parseFloat(parts[2])
  return hours * 3600 + minutes * 60 + seconds
}
function secondsToTime(seconds) {
  const h = Math.floor(seconds / 3600)
  const m = Math.floor((seconds % 3600) / 60)
  const s = (seconds % 60).toFixed(3)
  const hh = h.toString().padStart(2, '0')
  const mm = m.toString().padStart(2, '0')
  const ss = s.toString().padStart(6, '0')
  return `${hh}:${mm}:${ss}`
}

let currentUpdateHandler = null

function playPart(index, mainStartTime, endTime, startTime, rePlayed = false) {
  if (!player.value) return;

  const startSec = timeToSeconds(startTime)
  const endSec = timeToSeconds(endTime)
  const mainStartSec = timeToSeconds(mainStartTime)

  if (playingIndex.value === index && !rePlayed) {
    if (currentUpdateHandler) {
      player.value.removeEventListener('timeupdate', currentUpdateHandler)
      currentUpdateHandler = null
    }

    data[index].startTime = secondsToTime(player.value.currentTime.toFixed(3))
    player.value.pause()
    playingIndex.value = null

    return
  }


  if (currentUpdateHandler) {
    player.value.removeEventListener('timeupdate', currentUpdateHandler)
    currentUpdateHandler = null
  }

  playingIndex.value = index
  player.value.currentTime = startSec
  player.value.play()

  const updateHandler = () => {
    if (player.value.currentTime >= endSec) {
      data[index].startTime = mainStartTime

      player.value.pause()
      playingIndex.value = null

      if (currentUpdateHandler) {
        player.value.removeEventListener('timeupdate', currentUpdateHandler)
        currentUpdateHandler = null
      }
    }
  }
  currentUpdateHandler = updateHandler
  player.value.addEventListener('timeupdate', updateHandler)

}

function rePlay(index, mainStartTime, endTime, startTime) {
  playPart(index, mainStartTime, endTime, mainStartTime, true)
}

</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
