<template>
  <div class="home-page">
    <h1>Калькулятор подсетей</h1>
    <NetworkCalculator />
  </div>
</template>

<script setup lang="ts">
import NetworkCalculator from '../components/NetworkCalculator/NetworkCalculator.vue'
</script>

<style scoped>
.home-page {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}
</style>