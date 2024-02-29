<script setup>

var lstIconSimple = [
  "dove", "dove",
  "cow", "cow",
  "hippo", "hippo",
  "otter", "otter",
  "dragon", "dragon",
  "shrimp", "shrimp",
  "locust", "locust",
  "frog", "frog"
]
var lstIcons = [...lstIconSimple, ...lstIconSimple]
var shuffleLstIcons = lstIcons.sort(() => (Math.random() > .5) ? 1 : -1);

const emit = defineEmits(['goHome']);

var displayMode = (mode) => {
  emit('goHome',mode);
};

var lstIconsClickede = [];
var cardClicked = 0;
var pairDone = 0;

function cardSelect(event) {
  let card = event.currentTarget;
  
  if (card.classList[1] != 'validate') {
    if (card.classList[1] != 'active') {
      if (cardClicked < 2) {
        card.classList.add('active');
        if (card.childNodes[0].tagName === 'INPUT') {
          lstIconsClickede.push(card.childNodes[0].value);
        }
        cardClicked++;
      }
      if (cardClicked == 2) {
        var lstCardClicked = document.getElementsByClassName('active');
        setTimeout(function() {
            if (cardClicked != 2) {return;}
            if (lstIconsClickede[0] == lstIconsClickede[1]) {
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
            lstIconsClickede = [];
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

</script>

<template>
  <div class="bodyDiv">
    <div class="infiniteDiv">
      <div class="headerInfiniteDiv">
        <p class="goHome" @click="displayMode(null)">Back</p>
        <h3>Infinite Mode</h3>
        <p>Reload</p>
      </div>
      <div class="bodyInfiniteDiv">
        <template v-for="n in 16" :key="n">
          <div :id="`card-${n}`" class="card" @click="cardSelect($event)">
            <input type="hidden" name="icon" :value=shuffleLstIcons[n-1]>
            <div class="card-side back">
              <span class="cardBack">?</span>
            </div>
            <div class="card-side front">
              <span>
                <font-awesome-icon :icon="['fas', shuffleLstIcons[n-1]]" />
              </span>
            </div>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<style lang="scss">@import './InfiniteStyle.scss';</style>