<script setup lang="ts">
import SphereEntity from '../components/SphereEntity.vue'
import Windows from '../components/Windows.vue'
</script>

<template>
  <div class="container">
    <SphereEntity />
    <Windows />
  </div>
</template>

<style>
.container {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}
</style>
