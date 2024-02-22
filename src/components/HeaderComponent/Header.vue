<template>
  <div class="headerDiv">
    <h1 class="titleApp" @click="displayMode(null)">Where Is The Other ?</h1>
  </div>
</template>

<script setup>
  const emit = defineEmits(['goHome']);
  
  const displayMode = (mode) => {
    emit('goHome',mode);
  };
</script>

<style lang="scss">@import './HeaderStyle.scss';</style>