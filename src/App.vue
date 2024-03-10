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
  <div class="notAvailable">
    <p><span>Where Is The Other ?</span> is not available on this size of screen.</p>
    <p>Please try on bigger screen.</p>
  </div>
  <div class="bodyDiv">
    <menu-component v-if="gameValue === null" @goHome="setMode"></menu-component>
    <infinite-component v-if="gameValue === 'infinite'" @goHome="setMode"></infinite-component>
    <chrono-component v-if="gameValue === 'chrono'" @goHome="setMode"></chrono-component>
  </div>
  <footer-component></footer-component>
</template>

<style scoped>
</style>
