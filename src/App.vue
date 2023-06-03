<script setup>
import { RouterView } from "vue-router";
import HeaderTodo from "./components/HeaderTodo.vue";
</script>

<template>
  <HeaderTodo />
  <RouterView />
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Krub:ital,wght@1,300&family=Nunito+Sans:wght@200&family=Rubik:wght@300;500;700&display=swap");
* {
  font-family: "Rubik", sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  max-width: 1100px;
  margin: 0 auto;
}
</style>
