<template>
  <div>
    <canvas ref="canvas" :width="width" :height="height"></canvas>
    <BasicEnemy
        v-for="(enemy, index) in enemies"
        :key="index"
        :index="index"
        :fighterX="enemy.fighterX"
        :projectiles="projectiles"
         @hit="handleEnemyHit(index)"
        @removeProjectile="handleRemoveProjectile"
    />
    <GroundFighter @move="handleFighterMove" @projectilesUpdated="updateProjectiles" />  </div>
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
      enemies: Array.from({ length: 8 }, (_, i) => ({
        fighterX: i * 100, // Adjust this value to set the initial position of each BasicEnemy
      })),
      enemyWidth: 50, // width of each enemy
      fighterX: 0, // initial x position of the GroundFighter
      projectiles: [], // array to hold the projectiles
    };
  },
  mounted() {
    this.drawStars();
  },
  methods: {
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
        ctx.fillStyle = 'white';
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
    }
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