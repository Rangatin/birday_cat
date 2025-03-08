<script setup lang="ts">

import { onMounted, ref } from 'vue';

// Reactive state to track the flap status
const isFlapped = ref(false);

// Method to toggle the flap state
const toggleFlap = () => {
  isFlapped.value = !isFlapped.value;
};

onMounted(() => {
  // Set to true after a short delay to allow the component to render
  setTimeout(() => {
    isFlapped.value = true; // Trigger the flap animation
  }, 100); // Adjust the delay as needed
});
</script>

<template>
  <div class="container">
    <div class="envelope-wrapper" @click="toggleFlap" :class="{ flap: isFlapped }">
      <div class="envelope">
        <div class="letter">
          <div class="text">
            <strong>Miela Laurita,</strong>
            <p style="text-align: justify;">
              Sveikinimai gimtadienio proga iš saulėtų Alpių! Tegul gyvenime sekasi taip gerai kaip su
              slidinėjimu (O sekasi tikrai gerai!). Linkiu išlikti tokiai pat šauniai, supratingai ir palaikančiai!
            </p>
            <br />
            <p style="text-align: center;">Justinas</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  --envelope-primary: rgb(243, 205, 116);
  --envelope-secondary: rgb(232, 220, 192);
  --envelope-background: rgb(241, 174, 19);
}

.container {
  height: 100%;
  display: grid;
  place-items: center;
}

.container>.envelope-wrapper {
  background: var(--envelope-background);
  box-shadow: 0 0 40px gray;
}

.envelope-wrapper>.envelope {
  position: relative;
  width: 300px;
  height: 230px;
}

.envelope-wrapper>.envelope::before {
  content: "";
  position: absolute;
  top: 0;
  z-index: 2;
  border-top: 130px solid var(--envelope-secondary);
  border-right: 150px solid transparent;
  border-left: 150px solid transparent;
  transform-origin: top;
  transition: all 0.5s ease-in-out 0.7s;
}

.envelope-wrapper>.envelope::after {
  content: "";
  position: absolute;
  z-index: 2;
  width: 0px;
  height: 0px;
  border-top: 130px solid transparent;
  border-right: 150px solid var(--envelope-primary);
  border-bottom: 100px solid var(--envelope-primary);
  border-left: 150px solid var(--envelope-primary);
}

.envelope>.letter {
  position: absolute;
  right: 20%;
  bottom: 0;
  width: 60%;
  height: 100%;
  background: white;
  text-align: center;
  transition: all 1s ease-in-out;
  box-shadow: 0 0 5px gray;
  padding: 20px;
}

.envelope>.letter>.text {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  color: black;
  text-align: left;
  font-size: 10px;
  justify-content: center;
}



.flap>.envelope:before {
  transform: rotateX(180deg);
  z-index: 0;
}

.flap>.envelope>.letter {
  bottom: 100px;
  transform: scale(1.5);
  transition-delay: 1s;
}

.flap>.heart {
  transform: rotate(90deg);
  transition-delay: 0.4s;
}
</style>
