<script setup lang="ts">
import FormView from './views/FormView.vue'
</script>

<template>
  <main>
    <FormView />
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 100vw;
  min-height: 100vh;
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
