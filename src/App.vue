<script setup>
import { ref, onMounted } from 'vue'

const transcript = ref('')
const isRecording = ref(false)

const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition
const sr = new Recognition()

onMounted(() => {
  sr.continous = true
  sr.interminResults = true

  sr.onstart = () => {
    console.log("SR started")
    isRecording.value = true
  }

   sr.onend = () => {
    console.log("SR stoped")
    isRecording.value = false
  }

    sr.onresult = (evt) => {
      console.log(evt)
    }

});

const ToggleMic = () => {
  if (isRecording.value) {
    sr.stop()
  }
  else {
    sr.start()
  }
}
</script>

<template>
  <div>
    <h1>Hello</h1>
    <button @click="ToggleMic()">Start</button>
  </div>
</template>

<style>
  * {
  margin: 0;
  padding: 0;
  font-family: 'Fira Sans', sans-serif;
  box-sizing: border-box;
}
body {
  background: #281936;
  color: #fff;
}
</style>
