<template>
  <div class="splash-screen" v-if="show">
 <pre>

 / ___|  __ _| | ___  ___ / _| ___  _ __ ___ ___
 \___ \ / _` | |/ _ \/ __| |_ / _ \| '__/ __/ _ \
  ___) | (_| | |  __/\__ \  _| (_) | | | (_|  __/
 |____/ \__,_|_|\___||___/_|  \___/|_|  \___\___|
 |_ _|_ ____   ____ _  __| | ___ _ __ ___
  | || '_ \ \ / / _` |/ _` |/ _ \ '__/ __|
  | || | | \ V / (_| | (_| |  __/ |  \__ \
 |___|_| |_|\_/ \__,_|\__,_|\___|_|  |___/


    </pre>
    <h4>A game by Jared Odulio</h4>
    <h2>Press Any Key To Start</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: true, // Show the splash screen initially
    };
  },
  props: {

    gameStarted: {
      type: Boolean,
      default: false,
    },
  },
  mounted() {
    window.addEventListener('keydown', this.startGame);

  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.startGame);
  },
  methods: {
    startGame() {
      console.log('Game started')
      this.show = false; // Hide the splash screen
      this.$emit('start'); // Emit the start event
    },
  },
};
</script>

<style scoped>
.splash-screen {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: black; /* Change this line */
  color: white;
  font-size: 2em;
}
.splash-screen h2 {
  font-family: 'Press Start 2P', cursive;
}
.splash-screen pre {
  background-color: black;
  color: yellow; /* Change text color to black for better visibility */
}
</style>