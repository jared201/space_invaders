<template>
  <div class="basic-enemy" :style="{ left: x + 'px', top: y + 'px' }" v-if="!isHit">
    <svg class="blinking-enemy" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="pink" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M12 2L2 7l10 5 10-5-10-5z"></path>
      <path d="M2 7v7l10 5 10-5V7"></path>
      <path d="M2 14l10 5 10-5"></path>
      <path d="M12 22l10-5"></path>
      <path d="M22 12l-10 5-10-5"></path>
      <path d="M2 12l10-5 10 5"></path>
    </svg>
  </div>
</template>
<script>
export default {
  props: ['fighterX', 'projectiles', 'index'], // position of the GroundFighter
  data() {
    return {
      x: 0, // initial x position
      y: 0, // initial y position
      direction: 'right', // initial direction
      counter: 0, // counter for vertical movement
      isHit: false, // Add isHit data property
    };
  },
  methods: {
    move() {
      if (this.direction === 'right') {
        this.x += 10;
      } else {
        this.x -= 10;
      }
      this.counter++;
      if (this.counter === 5) {
        this.y += 10;
        this.counter = 0;
        this.direction = this.direction === 'right' ? 'left' : 'right';
      }
      if (this.y > window.innerHeight) {
        this.y = 0;
      }
      if (this.x > window.innerWidth) {
        this.x = 0;
      }
      //follow the GroundFighter
      if (this.x < this.fighterX) {
        this.x += 5;
      } else {
        this.x -= 5;
      }
      // let enemies fly in random directions
      if (Math.random() > 0.95) {
        this.direction = Math.random() > 0.5 ? 'right' : 'left';
      }
      //add a collision detection that works when the projectile hits the enemy

      for (let i = 0; i < this.projectiles.length; i++) {
        const projectile = this.projectiles[i];
        const enemyBox = { x: this.x, y: this.y, width: this.$el.offsetWidth, height: this.$el.offsetHeight };
        const projectileBox = { x: projectile.x, y: projectile.y, width: projectile.width, height: projectile.height };


        if (
            enemyBox.x < projectileBox.x + projectileBox.width &&
            enemyBox.x + enemyBox.width > projectileBox.x &&
            enemyBox.y < projectileBox.y + projectileBox.height &&
            enemyBox.y + enemyBox.height > projectileBox.y
        ) {
          this.isHit = true; // Mark the BasicEnemy as hit
          this.$emit('removeProjectile', i); // i is the index of the projectile to be removed
          this.$emit('hit', { enemyIndex: this.index, projectileIndex: i }); // Emit a hit event with the index of the hit enemy and projectile
          break;
        }
      }


    },
  },
  mounted() {
    // Call the move method every 100ms
    this.interval = setInterval(() => this.move(), 100);
  },
  beforeDestroy() {
    // Clear the interval when the component is destroyed
    clearInterval(this.interval);
  },
};
</script>


<style scoped>
.basic-enemy {
  position: absolute;
  width: 50px;
  height: 50px;
}

.blinking-enemy {
  animation: blink 1s linear infinite;
}

@keyframes blink {
  0% {
    fill: pink;
  }
  50% {
    fill: blue;
  }
  100% {
    fill: pink;
  }
}
</style>