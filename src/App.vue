<script setup lang="ts">
import Calculator from './components/Calculator.vue'
</script>

<template>
  <div class="page">
    <h1>Board Cut Calculator</h1>
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
  padding: 0 0 0.5em 0;
  margin: 0;
}
</style>
