<script setup>
// IMPORT
import CARD from '../CardComponent/CARD';
import { onMounted, nextTick, ref } from 'vue';

// COMPONENTS
import CardComponent from '../CardComponent/Card.vue';
import VictoryComponent from '../VictoryComponent/Victory.vue';

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

/**
 * Allow user to shuffle card.
 */
function reloadGame() {
  reset();
  makeDataCard();
}


// GAME FUNCTION
var Msg = ref("");
var cardClicked = 0;
var pairDone = 0;
var lstIconsClicked = [];

const cardSelect = (event) => {
  let card = event.currentTarget;
  start();
  
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
                stop();
                victory();
                pairDone = 0;
              }
            } else {
              lstCardClicked[1].classList.remove('active')
              lstCardClicked[0].classList.remove('active')
            }
            lstIconsClicked = [];
            lstCardClicked = [];
            cardClicked = 0;
        }, 800);
      }
    } else {
      card.classList.remove('active');
      cardClicked--;
    }
  }
}


// CHRONOMETER
var clock = ref({
  el: '#clock',
  data: {
    time: '00:00.000'
  }
});

var timeBegan = null
, timeStopped = null
, stoppedDuration = 0
, started = null
, running = false;


const start = () => {
  if(running) return;
  
  if (timeBegan === null) {
    reset();
    timeBegan = new Date();
  }
  
  if (timeStopped !== null) {
    stoppedDuration += (new Date() - timeStopped);
  }
  
  started = setInterval(clockRunning, 10);	
  running = true;
}

const stop = () => {
  running = false;
  timeStopped = new Date();
  clearInterval(started);
}

const reset = () => {
  running = false;
  clearInterval(started);
  stoppedDuration = 0;
  timeBegan = null;
  timeStopped = null;
  clock.value.data.time = "00:00.000";
}

const clockRunning = () => {
  var currentTime = new Date()
  , timeElapsed = new Date(currentTime - timeBegan - stoppedDuration)
  , min = timeElapsed.getUTCMinutes()
  , sec = timeElapsed.getUTCSeconds()
  , ms = timeElapsed.getUTCMilliseconds();

  clock.value.data.time = 
    zeroPrefix(min, 2) + ":" + 
    zeroPrefix(sec, 2) + "." + 
    zeroPrefix(ms, 3);
};

const zeroPrefix = (num, digit) => {
  var zero = '';
  for(var i = 0; i < digit; i++) {
    zero += '0';
  }
  return (zero + num).slice(-digit);
}


// VICTORY MODAL
const victory = () => {
  Msg.value = "You have won ";
  var modalWin = document.getElementById('vicoryModalNode');
  modalWin.style.display = 'flex';
}

function closeVictory() {
  var modalWin = document.getElementById('vicoryModalNode');
  modalWin.style.display = 'none';
  reloadGame();
}


// ON
onMounted(makeDataCard);

</script>

<template>
  <div class="infiniteDiv">
    <div class="headerInfiniteDiv">
      <p class="goHome" @click="displayMode(null)">Back</p>
      <h3>Infinite Mode</h3>
      <p class="reload" @click="reloadGame()">Reload</p>
    </div>
    <div class="bodyGame">
      <victory-component :Msg="Msg" @closeVictory="closeVictory"></victory-component>
      <div class="bodyInfiniteDiv">
        <card-component v-for="(card, index) in data_card" :key="index" :carte="card" :index="index" @cardSelect="cardSelect"></card-component>
      </div>
    </div>
  </div>
</template>

<style lang="scss">@import './InfiniteStyle.scss';</style>