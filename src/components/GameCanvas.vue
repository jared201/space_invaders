<template>
  <div>
    <canvas ref="canvas" :width="width" :height="height"></canvas>
    <BasicEnemy v-for="(enemy, index) in enemies" :key="index" :style="{ left: getEnemyPosition(index) + 'px' }" />
    <GroundFighter />
  </div>
</template>

<script>
import GroundFighter from './sprites/GroundFighter.vue';
import BasicEnemy from "./sprites/BasicEnemy.vue";

export default {
  components: {
    BasicEnemy,
    GroundFighter,
  },
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      stars: 1000, // number of stars
      enemies: Array(8).fill(null), // array of 8 enemies
      enemyWidth: 50, // width of each enemy
    };
  },
  mounted() {
    this.drawStars();
  },
  methods: {
    drawStars() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');

      // Fill the canvas with a black background
      ctx.fillStyle = 'black';
      ctx.fillRect(0, 0, this.width, this.height);

      // Draw white dots as stars
      for (let i = 0; i < this.stars; i++) {
        const x = Math.random() * this.width;
        const y = Math.random() * this.height;
        const radius = Math.random() * 1.5;

        ctx.beginPath();
        ctx.arc(x, y, radius, 0, Math.PI * 2, false);
        ctx.fillStyle = 'white';
        ctx.fill();
      }
    },
    getEnemyPosition(index) {
      const middle = this.width / 2;
      const totalEnemiesWidth = this.enemies.length * this.enemyWidth;
      const startPosition = middle - totalEnemiesWidth / 2;
      return startPosition + index * this.enemyWidth;
    },
  },
};
</script>

<style scoped>
canvas {
  position: absolute;
  top: 0;
  left: 0;
}
</style>