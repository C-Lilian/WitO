<template>
  <h2>Menu :</h2> 
  <div class="btnMode">
    <button class="btnMode activeBtn" @click="displayMode('infinite')">Infinite Mode</button>
    <button class="btnMode activeBtn" @click="displayMode('chrono')">Chrono Mode</button>
    <!--<button class="btnMode notActiveBtn" style="cursor:not-allowed">...</button>-->
  </div>
</template>

<script setup>

const emit = defineEmits(['goHome']);

const displayMode = (mode) => {
  emit('goHome',mode);
};

</script>

<style lang="scss">@import './MenuStyle.scss';</style>