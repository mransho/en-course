<template>
  x
  <audio ref="player" src="/1_Intro_MAIN.mp3"></audio>
  x
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
},
{
  text: "But the content, the focus, the topics in these lessons are going to be focused on the psychology of learning and the psychology of success.",
  mainStartTime: "00:00:43.050",
  startTime: "00:00:43.050",
  endTime: "00:00:53.670"
},
{
  text: "Now, these ideas, they're not just my ideas.",
  mainStartTime: "00:00:54.250",
  startTime: "00:00:54.250",
  endTime: "00:00:57.011"
},
{
  text: "In fact, they come from a lot of other people.",
  mainStartTime: "00:00:57.011",
  startTime: "00:00:57.011",
  endTime: "00:00:58.930"
},
{
  text: "They come from people like Tony Robbins, who's a famous success coach, Robert Anton Wilson, Joe Vitale, Robert Kiyosaki, Alan Watts.",
  mainStartTime: "00:00:58.930",
  startTime: "00:00:58.930",
  endTime: "00:01:08.790"
},
{
  text: "So, I've taken a lot of ideas from a lot of different people about this topic of the psychology of success.",
  mainStartTime: "00:01:08.790",
  startTime: "00:01:08.790",
  endTime: "00:01:15.071"
},
{
  text: "And the reason that we're going to talk about this in these lessons is that, in my experience, most English students struggle and fail because of psychology.",
  mainStartTime: "00:01:17.210",
  startTime: "00:01:17.210",
  endTime: "00:01:30.210"
},
{
  text: "Not because of methods, not because of teachers.",
  mainStartTime: "00:01:30.570",
  startTime: "00:01:30.570",
  endTime: "00:01:36.810"
},
{
  text: "Those things are important.",
  mainStartTime: "00:01:36.810",
  startTime: "00:01:36.810",
  endTime: "00:01:38.370"
},
{
  text: "But, you know, Tony Robbins talks about the fact that psychology is 80% of success.",
  mainStartTime: "00:01:38.510",
  startTime: "00:01:38.510",
  endTime: "00:01:45.470"
},
{
  text: "And I think he's right, based on my experience with many, many English students.",
  mainStartTime: "00:01:45.790",
  startTime: "00:01:45.790",
  endTime: "00:01:51.290"
},
{
  text: "Psychology is 80% of success.",
  mainStartTime: "00:01:51.990",
  startTime: "00:01:51.990",
  endTime: "00:01:54.071"
},
{
  text: "So, 20% are the methods you use to study.",
  mainStartTime: "00:01:54.071",
  startTime: "00:01:54.071",
  endTime: "00:01:57.710"
},
{
  text: "20% are the schools you go to, the teachers you have, the books you use.",
  mainStartTime: "00:01:57.710",
  startTime: "00:01:57.710",
  endTime: "00:02:03.450"
},
{
  text: "Those are important. We talk a lot about them.",
  mainStartTime: "00:02:03.450",
  startTime: "00:02:03.450",
  endTime: "00:02:05.850"
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
