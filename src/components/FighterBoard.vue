<script setup>

import GroundFighter from "@/components/sprites/GroundFighter.vue";
</script>

<template>
  <div class="fighter-grid">
  <GroundFighter/>
  </div>
</template>

<style scoped>
.fighter-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(50px, 1fr));
  grid-gap: 10px;
  padding: 20px;
  border-radius: 20px;
  background-color: #f0f0f0;
}
</style>