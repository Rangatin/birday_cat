<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';

import EnvelopeModal from './components/EnvelopeModal.vue';
import AudioPlayer from './components/AudioPlayer.vue';

// Reactive state to control modal visibility
const showModal = ref(false);
const playAudio = ref(false);

const openEnvelope = () => {
  showModal.value = true
  playAudio.value = true
  fireConfetti()
};


//-----------Var Inits--------------
const canvas = ref<HTMLCanvasElement | null>(null);
const ctx = ref<CanvasRenderingContext2D | null>(null);


const confetti: Array<{
  color: { front: string; back: string };
  dimensions: { x: number; y: number };
  position: { x: number; y: number };
  rotation: number;
  scale: { x: number; y: number };
  velocity: { x: number; y: number };
}> = [];

const confettiCount = 500;
const gravity = 0.5;
const terminalVelocity = 5;
const drag = 0.075;
const colors = [
  { front: 'red', back: 'darkred' },
  { front: 'green', back: 'darkgreen' },
  { front: 'blue', back: 'darkblue' },
  { front: 'yellow', back: 'darkyellow' },
  { front: 'orange', back: 'darkorange' },
  { front: 'pink', back: 'darkpink' },
  { front: 'purple', back: 'darkpurple' },
  { front: 'turquoise', back: 'darkturquoise' },
];

//-----------Functions--------------
const resizeCanvas = () => {
  if (canvas.value) {
    canvas.value.width = window.innerWidth;
    canvas.value.height = window.innerHeight;
  }
};

const randomRange = (min: number, max: number): number => Math.random() * (max - min) + min;

const initConfetti = () => {
  for (let i = 0; i < confettiCount; i++) {
    confetti.push({
      color: colors[Math.floor(randomRange(0, colors.length))],
      dimensions: {
        x: randomRange(10, 20),
        y: randomRange(10, 30),
      },
      position: {
        x: randomRange(0, canvas.value!.width),
        y: canvas.value!.height - 1,
      },
      rotation: randomRange(0, 2 * Math.PI),
      scale: {
        x: 1,
        y: 1,
      },
      velocity: {
        x: randomRange(-25, 25),
        y: randomRange(0, -50),
      },
    });
  }
};

//---------Render-----------
const render = () => {
  if (!ctx.value) return;

  ctx.value.clearRect(0, 0, canvas.value!.width, canvas.value!.height);

  confetti.forEach((confetto, index) => {
    const width = confetto.dimensions.x * confetto.scale.x;
    const height = confetto.dimensions.y * confetto.scale.y;

    // Move canvas to position and rotate
    ctx.value!.translate(confetto.position.x, confetto.position.y);
    ctx.value!.rotate(confetto.rotation);

    // Apply forces to velocity
    confetto.velocity.x -= confetto.velocity.x * drag;
    confetto.velocity.y = Math.min(confetto.velocity.y + gravity, terminalVelocity);
    confetto.velocity.x += Math.random() > 0.5 ? Math.random() : -Math.random();

    // Set position
    confetto.position.x += confetto.velocity.x;
    confetto.position.y += confetto.velocity.y;

    // Delete confetti when out of frame
    if (confetto.position.y >= canvas.value!.height) confetti.splice(index, 1);

    // Loop confetto x position
    if (confetto.position.x > canvas.value!.width) confetto.position.x = 0;
    if (confetto.position.x < 0) confetto.position.x = canvas.value!.width;

    // Spin confetto by scaling y
    confetto.scale.y = Math.cos(confetto.position.y * 0.1);
    ctx.value!.fillStyle = confetto.scale.y > 0 ? confetto.color.front : confetto.color.back;

    // Draw confetti
    ctx.value!.fillRect(-width / 2, -height / 2, width, height);

    // Reset transform matrix
    ctx.value!.setTransform(1, 0, 0, 1, 0, 0);
  });


  if (confetti.length <= 10) initConfetti();
  window.requestAnimationFrame(render);
};

const fireConfetti = () => {
  initConfetti(); // Initialize confetti
  render(); // Start rendering
};

onMounted(() => {
  if (canvas.value) {
    ctx.value = canvas.value.getContext('2d');
    resizeCanvas();

    //----------Resize----------
    window.addEventListener('resize', resizeCanvas);
  }

});

// Cleanup on unmount
onBeforeUnmount(() => {
  window.removeEventListener('resize', resizeCanvas);
});
</script>

<template>
  <canvas id="birthday" ref="canvas"></canvas>
  <div class="wrapper">
    <img alt="Laurita's birthday cat" class="cat" src="./assets/cat_hat.jpg" @click="openEnvelope">
    <AudioPlayer :play="playAudio" />
  </div>
  <EnvelopeModal v-model="showModal" />
</template>

<style scoped>
.wrapper {
  /* Create a positioning context */
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  z-index: 1;
}

.canvas {
  position: fixed;
  /* Position the canvas absolutely */
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: -1;
  /* Place canvas behind other elements */
}

.cat {
  display: block;
  width: clamp(50px, 50%, 400px);
  border-radius: 32px;
  z-index: 3;
}

.AudioPlayer {
  position: relative;
  /* Ensure audio player is above the canvas */
  z-index: 3;
  /* Higher z-index than the canvas */
}
</style>
