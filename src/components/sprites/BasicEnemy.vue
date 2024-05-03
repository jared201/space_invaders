<template>
  <div class="basic-enemy" :style="{ left: x + 'px', top: y + 'px' }">
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
  props: ['fighterX'], // position of the GroundFighter
  data() {
    return {
      x: 0, // initial x position
      y: 0, // initial y position
      direction: 'right', // initial direction
      counter: 0, // counter for vertical movement
    };
  },
  methods: {
    move() {
      const canvasWidth = this.$parent.width; // assuming the parent component has a width property

      // Determine direction based on the position of the GroundFighter
      if (this.x < this.fighterX) {
        this.direction = 'right';
      } else  if (this.x > this.fighterX) {
        this.direction = 'left';
      }

      // Move left or right depending on the direction
      if (this.direction === 'right') {
        this.x = Math.min(this.x + 10, canvasWidth - 50); // adjust this value to change the speed of horizontal movement
      } else {
        this.x = Math.max(this.x - 10, 0); // adjust this value to change the speed of horizontal movement
      }

      // Move down every second
      if (Date.now() % 1000 === 0) {
        this.y += 10; // adjust this value to change the speed of downward movement
        this.counter += 1; // increment the counter

        // Change direction after every 10 vertical movements
        if (this.counter % 10 === 0) {
          this.direction = this.direction === 'right' ? 'left' : 'right';
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