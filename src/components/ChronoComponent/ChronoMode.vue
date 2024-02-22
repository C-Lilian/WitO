<template>
  <div class="chronoBodyDiv">
    <div class="chronoDiv">
      <div class="headerChronoDiv">
        <p class="goHome" @click="displayMode(null)">Back</p>
        <h3>Chrono Mode</h3>
        <p>Reload</p>
      </div>
      <div class="bodyChronoDiv"></div>
    </div>
  </div>
</template>

<script setup>

const emit = defineEmits(['goHome']);

const displayMode = (mode) => {
  emit('goHome',mode);
};

</script>

<style lang="scss">@import './ChronoStyle.scss';</style>