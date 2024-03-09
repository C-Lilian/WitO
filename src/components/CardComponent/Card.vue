<script setup>
const props = defineProps({carte: Object});
const emit = defineEmits(['cardSelect'])

const cardSelect = (event) => {
  emit('cardSelect',event);
}
</script>

<template>
  <div :id="carte.name" class="card" @click="cardSelect($event)">
    <div class="card-side back">
      <span class="cardBack">?</span>
    </div>
    <div class="card-side front">
      <span><font-awesome-icon :icon="['fas', carte.name]" /></span>
    </div>
  </div>
</template>

<style lang="scss">@import './card.scss';</style>