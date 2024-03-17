<script setup>
// IMPORT
import jsPDFInvoiceTemplate from "jspdf-invoice-template";

const props = defineProps({Msg: String, Time:String});

const emit = defineEmits( ['closeVictory']);

// Close victory modal.
const closeVictory = () => {
  emit('closeVictory');
}

// Generate certificate.
const generatePDF = (winner_time) => {
  var temps = "";
  winner_time.Time === undefined ?  temps = "" : temps = ", completing it in record time "+winner_time.Time;
  var winner_name = prompt("Please enter your name for the certificate generation :");
  const props = {
                  outputType: "save",
                  returnJsPDFDocObject: true,
                  fileName: "WitO_Victory",
                  orientationLandscape: false,
                  compress: true,
                  business: {
                      name: "WitO",
                      address: "France",
                      website: "https://where-is-the-other.fr",
                  },
                  contact: {
                      label: "Victory certificate for :",
                      name: winner_name,
                  },
                  invoice: {
                      invDescLabel: "Congrats !",
                      invDesc: "Congratulations! We are thrilled to announce that you have emerged as the winner of our memory game"+temps+". WitO extends its warmest felicitations to you! We invite you to share your remarkable score with the world and to keep the excitement going by replaying the game. Your achievement truly inspires us all !",
                  },
                  footer: {
                      text: "© 2024 C-Lilian & WitO",
                  },
                  pageEnable: true,
                  pageLabel: "Page ",
              };
  
  jsPDFInvoiceTemplate(props);
  closeVictory();
} 
</script>

<template>
  <div id="vicoryModalNode" class="vicoryModalNode">
        <div class="modaleDiv">
          <h4>CONGRATS !</h4>
          <p>{{ Msg }} !</p>
          <div class="btnModal">
            <button class="activeBtnModal" title="Download" @click="generatePDF({Time})"><font-awesome-icon :icon="['fas', 'download']" /></button>
            <button class="activeBtnModal" title="Close" @click="closeVictory()"><font-awesome-icon :icon="['fas', 'xmark']" /></button>
          </div>
        </div>
      </div>
</template>

<style lang="scss">@import './VictoryStyle.scss';</style>