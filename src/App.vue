<script setup>

  import { ref , onMounted } from 'vue'

  const transcription = ref("");
  const isRecording = ref(false);

  const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  const SpRec = new Recognition();

  onMounted(() => {

    SpRec.continuous = true
    SpRec.interimResults = true

    SpRec.onstart = () => {
      isRecording.value = false
      console.log('start');
    };

    SpRec.onend = () => {
      isRecording.value = true
      console.log('stop');
    };

    SpRec.onresult = (event) => {
      var text = Array.from(event.results)
        .map(result => result[0])
        .map(result => result.transcript)
        .join("");
        transcription.value = text;
    };

  });

  const ToggleMic = () => {

    console.log(isRecording);

    if(isRecording.value == true){
      SpRec.stop();
    }else{
      SpRec.start();
    }
  }

</script>

<template>

  <Button :class="`mic`" @click="ToggleMic">
    Start
  </Button>

  <section class="SR_main">
    {{ transcription }}
  </section>

</template>



<style>

body {
    background: #151515;
}

section.SR_main {
    margin-top: 2rem;
    color: #fff;
    font-size: 2rem;
}

</style>
