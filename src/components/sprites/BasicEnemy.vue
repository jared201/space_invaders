<template>
  <div class="basic-enemy">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="pink" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M12 2L2 7l10 5 10-5-10-5z"></path>
      <path d="M2 7v7l10 5 10-5V7"></path>
      <path d="M2 14l10 5 10-5"></path>
      <path d="M12 22l10-5"></path>
      <path d="M22 12l-10 5-10-5"></path>
      <path d="M2 12l10-5 10 5"></path>
    </svg>
  </div>
</template>

<style scoped>
.basic-enemy {
  position: absolute;
  width: 50px;
  height: 50px;
}
</style>