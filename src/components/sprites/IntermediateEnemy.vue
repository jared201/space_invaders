<template>
  <div :style="{ left: x + 'px', top: y + 'px' }" class="intermediate-enemy">
    <svg width="30" height="30" viewBox="0 0 30 30" xmlns="http://www.w3.org/2000/svg">
      <path d="M4.5,10.5 C4.5,13.5 6.5,15.5 9,15.5 C11.5,15.5 13.5,13.5 13.5,10.5 C13.5,7.5 11.5,5.5 9,5.5 C6.5,5.5 4.5,7.5 4.5,10.5 Z M9,8.5 C7.6,8.5 6.5,9.4 6.5,10.5 C6.5,11.6 7.6,12.5 9,12.5 C10.4,12.5 11.5,11.6 11.5,10.5 C11.5,9.4 10.4,8.5 9,8.5 Z M25.5,10.5 C25.5,13.5 23.5,15.5 21,15.5 C18.5,15.5 16.5,13.5 16.5,10.5 C16.5,7.5 18.5,5.5 21,5.5 C23.5,5.5 25.5,7.5 25.5,10.5 Z M21,8.5 C19.6,8.5 18.5,9.4 18.5,10.5 C18.5,11.6 19.6,12.5 21,12.5 C22.4,12.5 23.5,11.6 23.5,10.5 C23.5,9.4 22.4,8.5 21,8.5 Z M12,20.5 C12,23.5 10,25.5 7.5,25.5 C5,25.5 3,23.5 3,20.5 C3,17.5 5,15.5 7.5,15.5 C10,15.5 12,17.5 12,20.5 Z M18,20.5 C18,23.5 20,25.5 22.5,25.5 C25,25.5 27,23.5 27,20.5 C27,17.5 25,15.5 22.5,15.5 C20,15.5 18,17.5 18,20.5 Z" fill="yellow" />
    </svg>
  </div>

</template>
<script>
export default {
  props: ['fighterX', 'projectiles', 'index'],
  data() {
    return {
      step: 1,
      x: '100',
      y: '100',
      direction: 'right',
    };
  },
  mounted() {
    setInterval(this.move, 100);
    setInterval(this.fireProjectile, 1000);
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
    },
    fireProjectile() {

      this.$emit('fire', {
        x: this.x,
        y: this.y,
      });
    },

  },
};
</script>
<style scoped>
.intermediate-enemy {
  position: absolute;
  transform: translate(-50%, -50%);
  width: 50px;
  height: 50px;
}
</style>
