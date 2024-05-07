<template>
  <div>
    <SplashScreen v-if="!gameStarted" @start="startGame" />
    <canvas class="star" ref="canvas" :width="width" :height="height"></canvas>
    <IntermediateEnemy
        v-for="(enemy, index) in intermediateEnemies"
        :key="index"
        :x="enemy.fighterX"
        :y="enemy.y"
        @move="handleEnemyMove(index)"
        @fire="handleEnemyFire(index)"
    />
    <BasicEnemy
        v-for="(enemy, index) in enemies"
        :key="index"
        :index="index"
        :fighterX="enemy.fighterX"
        :projectiles="projectiles"
        @hit="handleEnemyHit"
        @removeProjectile="handleRemoveProjectile"
    />
    <GroundFighter @move="handleFighterMove" @projectilesUpdated="updateProjectiles" />  </div>
</template>


<script>
import GroundFighter from './sprites/GroundFighter.vue';
import BasicEnemy from "./sprites/BasicEnemy.vue";
import SplashScreen from './SplashScreen.vue';
import IntermediateEnemy from "@/components/sprites/IntermediateEnemy.vue";

export default {
  components: {
    IntermediateEnemy,
    BasicEnemy,
    GroundFighter,
    SplashScreen,
  },
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      stars: 1000, // number of stars
      enemies: Array.from({ length: 8 }, (_, i) => ({
        fighterX: i * 100, // Adjust this value to set the initial position of each BasicEnemy
      })),
      enemyWidth: 50, // width of each enemy
      fighterX: 0, // initial x position of the GroundFighter
      projectiles: [], // array to hold the projectiles
      gameStarted: false, // Add this line
      intermediateEnemies: Array.from({ length: 8 }, (_, i) => ({
        x: i * 100,
        y: 100,
      })),
    };
  },
  mounted() {
    this.showSplashScreen();
    this.drawStars();
  },
  methods: { //add splash screen methods
    showSplashScreen() {
      this.gameStarted = false; // Set gameStarted to false when the game starts
      this.$refs.canvas.style.display = 'none';
    },
    startGame() {
      console.log('Game started');
      this.gameStarted = true; // Set gameStarted to true when the game starts
      this.$refs.canvas.style.display = 'block';
    },
    updateProjectiles(newProjectiles) {
      this.projectiles = newProjectiles;
    },
    handleRemoveProjectile(projectileIndex) {
      this.projectiles.splice(projectileIndex, 1);
    },
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
        ctx.fillStyle = 'yellow';
        ctx.fill();
      }
    },

    handleFighterMove(newX) {
      this.fighterX = newX;
      this.checkCollisions();

    },
    handleEnemyHit({ enemyIndex, projectileIndex }) {
      // Remove the hit BasicEnemy from the enemies array
      this.enemies.splice(enemyIndex, 1);

      // Remove the hit projectile from the projectiles array
      this.projectiles.splice(projectileIndex, 1);
    },
    checkCollisions() {
      for (let i = 0; i < this.enemies.length; i++) {
        for (let j = 0; j < this.projectiles.length; j++) {
          const enemy = this.enemies[i];
          const projectile = this.projectiles[j];
          const enemyBox = {x: enemy.x, y: enemy.y, width: this.enemyWidth, height: this.enemyWidth};
          const projectileBox = {x: projectile.x, y: projectile.y, width: projectile.width, height: projectile.height};

          if (
              enemyBox.x < projectileBox.x + projectileBox.width &&
              enemyBox.x + enemyBox.width > projectileBox.x &&
              enemyBox.y < projectileBox.y + projectileBox.height &&
              enemyBox.y + enemyBox.height > projectileBox.y
          ) {
            this.handleEnemyHit({enemyIndex: i, projectileIndex: j});
            break;
          }
        }
      }
    },
    handleEnemyMove(index) {
      this.enemies[index].x = Math.random() * this.width;
    },
    handleEnemyFire(index) {
      this.projectiles.push({
        x: this.enemies[index].x + this.enemyWidth / 2,
        y: this.enemies[index].y + this.enemyWidth,
        width: 5,
        height: 10,
      });
    },
  },

};
</script>

<style scoped>
.star {
  box-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #fff, 0 0 40px #fff;
  animation: glow 2s infinite;
}
canvas {
  position: absolute;
  top: 0;
  left: 0;
}
</style>