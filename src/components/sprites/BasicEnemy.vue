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
      //add a collision detection for the projectiles and the BasicEnemy

      for (let i = 0; i < this.projectiles.length; i++) {
        const dx = this.projectiles[i].x - this.x;
        const dy = this.projectiles[i].y - this.y;
        const distance = Math.sqrt(dx * dx + dy * dy);
        const radiusSum = 25; // sum of the radii of the projectile and the enemy

        if (distance < radiusSum) {
          this.isHit = true;
          this.$emit('hit', this.index);
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