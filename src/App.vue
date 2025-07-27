<script setup lang="ts">
import Calculator from './components/Calculator.vue'
</script>

<template>
  <div class="page">
    <h1>Board Cut Calculator</h1>
    <p>Paste in a list of cut lengths below and calculate how much stock you need to get the job done!</p>
    <Calculator />
  </div>
</template>

<style scoped>
.page {
  max-width: 1200px;
  margin: auto;
  width: 100%;
}
h1 {
  text-align: center;
  padding: 0 0 0.25em 0;
  margin: 0;
}
p {
  text-align: center;
  margin: 0 0 1em 0;
  padding: 0;
}
</style>
