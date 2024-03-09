<script setup>
// IMPORT
import CARD from '../CardComponent/CARD';
import { onMounted, nextTick, ref } from 'vue';

// COMPONENTS
import CardComponent from '../CardComponent/Card.vue';

// EMIT
const emit = defineEmits(['goHome']);

const displayMode = (mode) => {
  emit('goHome',mode);
};


class Carte {
  constructor (name, traduction) {
    this.name = name;
    this.traduction = traduction;
  }
}

let data_card = ref([]);

const makeDataCard = async () => {
  const shuffleLstIcons = CARD.sort(() => (Math.random() > .5) ? 1 : -1);
  data_card.value = [];
  await nextTick();
  for (const card of shuffleLstIcons) {
    const new_card = new Carte(card.name, card.traduction);
    
    data_card.value.push(new_card);
  }
}

function reloadGame() {
  makeDataCard();
}


var cardClicked = 0;
var pairDone = 0;
var lstIconsClicked = [];

const cardSelect = (event) => {
  let card = event.currentTarget;
  
  if (card.classList[1] != 'validate') {
    if (card.classList[1] != 'active') {
      if (cardClicked < 2) {
        card.classList.add('active');
        lstIconsClicked.push(card.id);
        cardClicked++;
      }
      if (cardClicked == 2) {
        var lstCardClicked = document.getElementsByClassName('active');
        setTimeout(function() {
            if (cardClicked != 2) {return;}
            if (lstIconsClicked[0] == lstIconsClicked[1]) {
              lstCardClicked[1].classList.add('validate')
              lstCardClicked[1].classList.remove('active')
              lstCardClicked[0].classList.add('validate')
              lstCardClicked[0].classList.remove('active')
              pairDone++;
              if (pairDone == 8) {
                alert("Bravo, vous avez gagné !")
              }
            } else {
              lstCardClicked[1].classList.remove('active')
              lstCardClicked[0].classList.remove('active')
            }
            lstIconsClicked = [];
            lstCardClicked = [];
            cardClicked = 0;
        }, 1000);
      }
    } else {
      card.classList.remove('active');
      cardClicked--;
    }
  }
}

onMounted(makeDataCard);


</script>

<template>
  <div class="bodyDiv">
    <div class="infiniteDiv">
      <div class="headerInfiniteDiv">
        <p class="goHome" @click="displayMode(null)">Back</p>
        <h3>Infinite Mode</h3>
        <p class="reload" @click="reloadGame()">Reload</p>
      </div>
      <div class="bodyInfiniteDiv">
        <card-component v-for="(card, index) in data_card" :key="index" :carte="card" :index="index" @cardSelect="cardSelect"></card-component>
      </div>
    </div>
  </div>
</template>

<style lang="scss">@import './InfiniteStyle.scss';</style>