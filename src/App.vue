<script setup>
  import { RouterLink, RouterView } from 'vue-router'
  import { ref } from 'vue'
  import HeaderComponent from './components/HeaderComponent/Header.vue'
  import MenuComponent from './components/MenuComponent/Menu.vue'
  import InfiniteComponent from './components/InfiniteComponent/InfiniteMode.vue'
  import ChronoComponent from './components/ChronoComponent/ChronoMode.vue'
  import FooterComponent from './components/FooterComponent/Footer.vue'
  
  const gameValue = ref(null)
  const setMode = (mode) => {
    gameValue.value = mode;
  };
</script>

<template>
  <header-component @goHome="setMode"></header-component>
  <menu-component v-if="gameValue === null" @selected="setMode"></menu-component>
  <infinite-component v-if="gameValue === 'infinite'" @goHome="setMode"></infinite-component>
  <chrono-component v-if="gameValue === 'chrono'" @goHome="setMode"></chrono-component>
  <footer-component></footer-component>
</template>

<style scoped>
</style>
