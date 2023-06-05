<script setup>
import { ref, onMounted } from 'vue'

const transcript = ref('')
const isRecording = ref(false)
const stoppedRecording = ref(true)

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
      if (stoppedRecording.value === false)
      {
          sr.start()
      }
  }

  sr.onresult = (evt) => {
    const t = Array.from(evt.results)
      .map(result => result[0])
      .map(result => result.transcript)
      .join('')
    console.log(t)
    transcript.value = `${transcript.value} ${t}`
  }
  if ('speechSynthesis' in window) {
    console.log("Web Speech API supported!")
  } else {
    alert("Web Speech API not supported :-(")
  }

});

const ToggleMic = () => {
  if (isRecording.value) {
    sr.stop()
  }
  else {
    sr.start()
  }
    stoppedRecording.value = !stoppedRecording.value;
}
</script>

<template>

    <section class="bg-gray-900 text-white">
        <div class="mx-auto max-w-screen-xl px-4 py-32 flex min-h-screen h-auto">
            <div class="mx-auto max-w-3xl text-center">
                <div class="flex flex-wrap justify-center gap-4">
                    <button  @click="ToggleMic" class="block w-full rounded border border-blue-600 bg-blue-600 px-12 py-3 text-sm font-medium text-white hover:bg-transparent hover:text-white focus:outline-none focus:ring active:text-opacity-75 sm:w-auto">
                        <span v-if="isRecording">Stop</span>
                        <span v-else>Start</span>
                    </button>
                </div>
                <div class="mt-4">
                    <p class="text-gray-300 text-xs" v-if="isRecording">Listening...</p>
                </div>
                <p class="bg-gradient-to-r  mt-8 from-green-300 via-blue-500 to-purple-600 bg-clip-text text-3xl font-extrabold text-transparent sm:text-5xl">
                   {{ transcript }}
                </p>
            </div>
        </div>
    </section>
</template>
