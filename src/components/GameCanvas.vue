<template>
  <div>
    <canvas ref="canvas" :width="width" :height="height"></canvas>
    <GroundFighter />
  </div>
</template>

<script>
import GroundFighter from './sprites/GroundFighter.vue';

export default {
  components: {
    GroundFighter,
  },
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      stars: 1000, // number of stars
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