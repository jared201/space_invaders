<template>
  <svg class="ground-fighter" :style="{ left: left + 'px' }" width="50" height="50" viewBox="0 0 50 50" xmlns="http://www.w3.org/2000/svg">
    <path d="M10 40 L20 40 L20 30 L30 30 L30 40 L40 40 L40 50 L10 50 Z" fill="#ffffff" />
  </svg>
</template>
<script>
export default {
  data() {
    return {
      left: 0, // initial position
    };
  },
  mounted() {
    window.addEventListener('keydown', this.moveGroundFighter);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.moveGroundFighter);
  },
  methods: {
    moveGroundFighter(event) {
      const step = 10; // change this value to make the sprite move faster or slower
      switch (event.key) {
        case 'ArrowLeft':
          this.left = Math.max(this.left - step, 0);
          break;
        case 'ArrowRight':
          this.left = Math.min(this.left + step, window.innerWidth - 50); // 50 is the width of the sprite
          break;
      }
    },
  },
};
</script>
<style scoped>
.ground-fighter {
  position: fixed;
  bottom: 30px;
  left: 0;
}
</style>