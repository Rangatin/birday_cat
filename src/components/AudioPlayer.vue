<template>
  <audio ref="audio" :src="audioSrc"></audio>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
import happyMeowday from '/happy_meowday.mp3';

const props = defineProps({
  play: {
    type: Boolean,
    required: true,
  },
});

const audioSrc = happyMeowday
const audio = ref<HTMLAudioElement | null>(null);

onMounted(() => {
  audio.value = new Audio(audioSrc);
  audio.value.loop = true;
  audio.value.volume = 0.2; // Set initial volume
});

watch(() => props.play, (newValue) => {
  if (audio.value) {
    if (newValue) {
      audio.value.play();
    } else {
      audio.value.pause();
    }
  }
});
</script>
