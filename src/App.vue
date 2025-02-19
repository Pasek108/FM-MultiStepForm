<script setup lang="ts">
import FormView from './views/FormView.vue'
</script>

<template>
  <main>
    <FormView />

    <div class="attribution">Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. Coded by <a href="https://pasek108.github.io/Resume">Artur Pas</a>.</div>
  </main>
</template>

<style scoped>
main {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 100vw;
  min-height: 100lvh;
  padding: 2rem 0;
}

.attribution {
  position: absolute;
  bottom: 0;
  font-size: 14px;
  text-align: center;
}

.attribution a {
  font-size: 14px;
  color: hsl(228, 45%, 44%);
}

@media only screen and (max-width: 750px) {
  main {
    display: block;
    padding: 1rem;
    padding-bottom: 7rem;
    position: relative;
  }
}
</style>
