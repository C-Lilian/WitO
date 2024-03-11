<script setup>
const props = defineProps({Msg: String});

const emit = defineEmits( ['closeVictory']);

const closeVictory = () => {
  emit('closeVictory');
}
</script>

<template>
  <div id="vicoryModalNode" class="vicoryModalNode">
        <div class="modaleDiv">
          <h4>CONGRATS !</h4>
          <p>{{ Msg }} !</p>
          <div class="btnModal">
            <button class="notActiveBtnModal" title="Download"><font-awesome-icon :icon="['fas', 'download']" /></button>
            <button class="activeBtnModal" title="Close" @click="closeVictory()"><font-awesome-icon :icon="['fas', 'xmark']" /></button>
          </div>
        </div>
      </div>
</template>

<style lang="scss">@import './VictoryStyle.scss';</style>