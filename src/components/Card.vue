<template>
    <section class="memory-game">
      <div class="memory-card" :data-framework="card.name"
        v-for="(card,i) in cards" :key="i" 
        :class="{ 'flipped': card.isflipped, 'matched': card.isMatched }"
        @click="(!card.isMatched && !card.isFlipped)? flipCard(card):null">
          <img v-if="card.isFlipped" :src="cardPath(card.name)" :alt="card.name" />
          <img v-else src="@/assets/img/js-badge.svg" alt="JS Badge" />
      </div>
    </section>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: ['cardsArray', 'reset'],
  emits: ['reduceCounter', 'winner'],
  watch: {
    reset: function () {
      this.cards.map(c => {
        c.isFlipped = false;
        c.isMatched = false;
      })
    }
  },
  data() {
    const winner = {
      matchesToWin: 6
    }
    const flippedCards = [];
    const cards = this.cardsArray;
    return { cards, flippedCards, winner }
  },
  methods: {
    cardPath(name) {
      return require('@/assets/img/' + name + '.svg')
    },
    flipCard(card) {
      this.$emit('reduceCounter');
      card.isFlipped = !card.isFlipped;
      if (this.flippedCards.length < 2) {
        this.flippedCards.push(card);
      }
      if (this.flippedCards.length === 2) {
        this.matchCard(card)
      }
    },
    matchCard(card) {
      if (this.flippedCards[0].name === card.name) {
        setTimeout(() => {
          this.flippedCards.forEach(c => c.isMatched = true);
          this.flippedCards = [];
          --this.winner.matchesToWin === 0 ? this.$emit('winner') : null;
        }, 600)
      } else {
        setTimeout(() => {
          this.flippedCards.forEach(c => c.isFlipped = false);
          this.flippedCards = [];
        }, 600)
      }
    }
  }
}
</script>

<style scoped>
.memory-game {
  width: 640px;
  height: 640px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  perspective: 1000px;
}
.matched {
  border: 1px solid green;
}
.memory-card {
  width: calc(25% - 10px);
  height: calc(33.333% - 10px);
  margin: 5px;
  position: relative;
  transform: scale(1);
  transform-style: preserve-3d;
  transition: transform .5s;
  box-shadow: 1px 1px 1px rgba(0,0,0,.3);
}
.memory-card img{
  width: 100%;
  height: 100%;
  padding: 20px;
  position: absolute;
  border-radius: 5px;
  background: #FFF29E;
}
</style>
