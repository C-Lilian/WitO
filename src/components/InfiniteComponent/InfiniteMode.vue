<template>
  <div class="bodyDiv">
    <div class="infiniteDiv">
      <div class="headerInfiniteDiv">
        <p class="goHome" @click="displayMode(null)">Back</p>
        <h3>Infinite Mode</h3>
        <p>Reload</p>
      </div>
      <div class="bodyInfiniteDiv"></div>
    </div>
  </div>
</template>

<script setup>

const emit = defineEmits(['goHome']);

const displayMode = (mode) => {
  emit('goHome',mode);
};

</script>

<style lang="scss">@import './InfiniteStyle.scss';</style>