<template>
 <div class="counter">
   <h1>{{ counter.countsLeft }} tries left</h1>
 </div>
  <Card v-if="counter.countsLeft > 0"
    :cardsArray="cardsArray"
    :reset="reset"
    @reduceCounter="reduceCounter"
    @winner="weGotWinner"/>
  <div v-else class="gameOver">
    <p>Game Over, you really did not manage with 60 tries :/ ?</p>
    <button @click="addCounts">Add 4 more tries ?</button>
  </div>
  <div v-if="winner" class="winner">
    <p>Booom a winner !</p>
    <button @click="resetGame">Start Over?</button>
  </div>
</template>

<script>
import Card from './components/Card.vue'

export default {
  name: 'App',
  components: {
    Card
  },
  data() {
    const reset = false;
    const winner = false;
    const counter = {
      countsLeft: 60
    }
    const names = ['angular', 'aurelia', 'backbone', 'ember', 'react', 'vue'];
    //to have random position every new game
    const cardsArray = this.arrayShuffle(this.genCardObj(names))

    return { cardsArray, counter, winner, reset }
  },
  methods: {
    genCardObj(array) {
      const cardsArray = [];
      array.forEach(n => {
        const cardObj = { name: n, isFlipped: false, isMatched: false };
        const cardObj2 = { name: n, isFlipped: false, isMatched: false };
        cardsArray.push(cardObj, cardObj2)
      })
      return cardsArray;
    },
    arrayShuffle(array) {
      let length = array.length;
      while (length) {
        const index = Math.floor(Math.random() * length--);
        const temp = array[length];
        array[length] = array[index];
        array[index] = temp;
      }
      return array;
    },
    reduceCounter() {
      this.counter.countsLeft--;
    },
    addCounts() {
      setTimeout(() => {
        this.counter.countsLeft += 4
      }, 800)
    },
    weGotWinner() {
      this.winner = true;
    },
    resetGame() {
      this.counter.countsLeft = 60;
      this.winner = false;
      this.reset = true;
    }
  }
}
</script>
<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.counter {
  margin: 50px auto;
  text-align: center;
  background: #FFF29E;
}
.winner {
  margin: 20px auto;
  padding: 10px;
  text-align: center;
  background: #FFF29E;
  color: red;
}
.winner button {
  padding: 5px;
  background: white;
  border: 2px solid green;
  border-radius: 8px;
  color: green;
  font: 900;
}
.gameOver {
  background: red;
  padding: 20px;
  text-align: center;
}
.gameOver p {
  margin-bottom: 20px;
}
.gameOver button {
  padding: 5px;
  border: 2px solid black;
  background: white;
  font-weight: bold;
  color: green;
  border-radius: 8px;
}
</style>
