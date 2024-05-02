<template>
  <div class="enemy-grid">
    <BasicEnemy />
    <IntermediateEnemy />
    <!-- Add more enemy components as needed -->
    <AdvancedEnemy />
  </div>
</template>

<script setup>
import BasicEnemy from "@/components/sprites/BasicEnemy.vue";
import IntermediateEnemy from "@/components/sprites/IntermediateEnemy.vue";
import AdvancedEnemy from "@/components/sprites/AdvancedEnemy.vue";
</script>

<style scoped>
.enemy-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(50px, 1fr));
  grid-gap: 10px;
  padding: 20px;
  border-radius: 20px;
  background-color: #f0f0f0;
}
</style>