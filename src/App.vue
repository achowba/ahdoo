<script setup>
import { RouterView } from 'vue-router'
import TodoHeader from './components/TodoHeader.vue'
</script>

<template>
  <TodoHeader/>
  <RouterView />
</template>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,300..900;1,300..900&display=swap');

* {
  box-sizing: border-box;
  font-family: "Rubik", sans-serif;
  margin: 0;
  padding: 0;
}

.container {
  margin: 0 auto;
  max-width: 1100px;
}
</style>
