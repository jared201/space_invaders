<template>
  <div>
    <svg class="ground-fighter" :style="{ left: left + 'px' }" width="50" height="50" viewBox="0 0 50 50" xmlns="http://www.w3.org/2000/svg">
      <path d="M10 40 L20 40 L20 30 L30 30 L30 40 L40 40 L40 50 L10 50 Z" fill="#ffffff" />
    </svg>
    <div v-for="(projectile, index) in projectiles" :key="index" class="projectile" :style="{ left: projectile.x + 'px', top: projectile.y + 'px' }"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      left: 0, // initial position
      projectiles: [], // array to hold the projectiles
    };
  },
  mounted() {
    window.addEventListener('keydown', this.moveGroundFighter);
    setInterval(this.moveProjectiles, 100); // move the projectiles every 100ms
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
        case ' ':
          this.fireProjectile();
          break;
      }
    },
    fireProjectile() {
      this.projectiles.push({x: this.left + 25, y: window.innerHeight - 80}); // 25 is half the width of the sprite, 80 is the height of the sprite plus a little extra
    },
    moveProjectiles() {
      for (let i = 0; i < this.projectiles.length; i++) {
        this.projectiles[i].y -= 5; // move the projectile up the screen
        if (this.projectiles[i].y < 0) {
          this.projectiles.splice(i, 1); // remove the projectile if it's off the screen
          i--; // adjust the index after removing an item
        }
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

.projectile {
  position: fixed;
  width: 5px;
  height: 10px;
  background-color: white;
}
</style>