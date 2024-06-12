<script setup>
import mainView from './view/mainView/index.vue'
</script>

<template>
  <div class="mainContainer">
    <mainView />
  </div>
</template>

<style scoped>
.mainContainer {
  width: 100%;
  min-width: 950px;
  height: 100vh;
  min-height: 760px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
