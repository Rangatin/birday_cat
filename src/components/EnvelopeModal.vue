<template>
  <div v-if="isVisible" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content" @click.stop>
      <EnvelopeComponent />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, defineProps } from 'vue';
import EnvelopeComponent from './EnvelopeComponent.vue';

// Define props with TypeScript types
const props = defineProps<{
  modelValue: boolean;
}>();



// Reactive state for modal visibility
const isVisible = ref(props.modelValue);

// Watch for changes in the modelValue prop
watch(() => props.modelValue, (newValue) => {
  isVisible.value = newValue;
});

// Close modal function
const closeModal = () => {
  isVisible.value = false;
  // emit('update:modelValue', false);
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: transparent;
  position: relative;
  max-width: 500px;
  width: 100%;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 16px;
  cursor: pointer;
}
</style>
