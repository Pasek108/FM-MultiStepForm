<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  step_number: Number,
})

const steps = ['Your info', 'Select plan', 'Add-ons', 'Summary']

const screen_width = ref(window.innerWidth)
window.addEventListener('resize', () => (screen_width.value = window.innerWidth))
</script>

<template>
  <div class="sidebar">
    <img :src="'/images/bg-sidebar-' + (screen_width > 750 ? 'desktop' : 'mobile') + '.svg'" alt="sidebar background" />

    <div class="steps">
      <div class="step-container" v-for="(step, index) in steps" :key="index" :class="{ active: index + 1 == step_number }">
        <div class="col">
          <div class="number">{{ index + 1 }}</div>
        </div>

        <div class="col">
          <div class="step">Step {{ index + 1 }}</div>
          <div class="name">{{ step }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.sidebar {
  position: relative;
  z-index: 0;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 18rem;
  min-width: 18rem;
}

img {
  width: 100%;
}

.steps {
  position: absolute;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding: 2rem;
}

.step-container {
  display: flex;
  gap: 1rem;
  color: hsl(231, 100%, 99%);
}

.col {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: start;
  text-transform: uppercase;
}

.number {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 2.25rem;
  height: 2.25rem;
  border: 1px solid hsl(231, 100%, 99%);
  border-radius: 999rem;
}

.step-container.active .number {
  color: black;
  background-color: hsl(206, 94%, 87%);
}

.step {
  font-weight: 100;
  font-size: 0.875rem;
  line-height: 1;
  color: hsl(229, 24%, 87%);
}

.name {
  letter-spacing: 0.075rem;
  font-weight: 500;
}

@media only screen and (max-width: 920px) {
  .sidebar {
    width: 16rem;
    min-width: 16rem;
  }
}

@media only screen and (max-width: 750px) {
  .sidebar {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    z-index: -1;
  }

  .step-container .col:nth-child(2) {
    display: none;
  }

  .steps {
    flex-direction: row;
    justify-content: center;
    gap: 1rem;
    width: 100%;
  }
}
</style>
