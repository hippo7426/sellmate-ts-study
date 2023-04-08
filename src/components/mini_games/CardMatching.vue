<template>
  <h2 style="text-align: center">같은 그림 맞추기</h2>
  <div class="cards">
    <div
      v-for="(card, index) in shuffledCards"
      :key="`${card.id}${index}`"
      :class="{ flipped: card.flipped }"
      @click="flipCard(card)"
    >
      <div class="front" v-show="card.flipped">
        <img :src="card.img" alt="card-front" />
      </div>
      <div class="back" v-show="!card.flipped">
        <img src="src/assets/card_back.jpg" alt="card-back" />
      </div>
    </div>
  </div>
  <button @click="resetGame" :disabled="!numMatchedCards">Reset</button>
</template>

<script lang="ts">
import { ref, watch, type Ref } from 'vue'

interface Card{
    id: string,
    img: string,
    flipped?: boolean,
}

const NUM_OF_CARD_PAIR = 8 as const

function generateRandomCards(): Card[] {
  return Array.from({ length: NUM_OF_CARD_PAIR }, (v, idx) => {
    const randomStr = Math.random().toString(36).substring(2, 12)
    return {
      id: randomStr,
      img: `https://picsum.photos/seed/${randomStr}/120/200`,
    }
  })
}

export default {
  name: 'CardMatchingGame',
  setup() {
    const shuffledCards:Ref<Card[]> = ref([])
    const flippedCard:Ref<Card | null> = ref(null)
    const numMatchedCards = ref(0)
    let canClick = true
    let clickCount = 0;

    // shuffle the cards
    const shuffleCards = () => {
      const cards = generateRandomCards()
      const shuffledCardSrc = [...cards, ...cards].sort(() => Math.random() - 0.5)
      shuffledCards.value = shuffledCardSrc.map((card) => ({
        ...card,
        flipped: false,
      }))
      flippedCard.value = null
      numMatchedCards.value = 0
    }

    // flip a card
    const flipCard = (card: Card) => {
      if (!canClick || card.flipped) return
    
      clickCount++;
      if (flippedCard.value) {
        // check for a match
        if (flippedCard.value.id === card.id) {
          // match
          card.flipped = true
          flippedCard.value.flipped = true
          flippedCard.value = null
          numMatchedCards.value += 2
        } else {
          // not a match
          card.flipped = true
          canClick = false
          setTimeout(() => {
            card.flipped = false
            flippedCard.value!.flipped = false
            flippedCard.value = null
            canClick = true
          }, 1000)
        }
      } else {
        // first card flipped
        card.flipped = true
        flippedCard.value = card
      }
    }

    const resetGame = () => {
      shuffleCards()
    }

    watch(numMatchedCards, (newVal) => {
      if (newVal === shuffledCards.value.length) {
        setTimeout(() => {
          if (confirm(`전부 맞추셨습니다! 카드를 초기화 하시겠습니까? (클릭수: ${clickCount})`)) {
            resetGame()
            clickCount = 0;
          }
        })
      }
    })

    shuffleCards();

    return {
      shuffledCards,
      numMatchedCards,
      flipCard,
      resetGame,
    }
  },
}
</script>

<style lang="scss">
.cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin: auto;
  width: 599px;

  & > div {
    width: 110px;
    height: 180px;
    margin: 5px;
    cursor: pointer;

    & > div {
      width: 100%;
      height: 100%;
      border-radius: 5px;
      transition: transform 0.5s;

      &.back > img {
        width: 100%;
        height: 100%;
        object-fit: contain;
      }

      &.front > img {
        width: 100%;
        height: 100%;
        border: 2px solid black;
        border-radius: 4px;
        object-fit: fill;
      }
    }
  }
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  border: 1px solid #3b82f6;
  background-color: #fff;
  color: #3b82f6;
  cursor: pointer;
  margin: auto;
  margin-top: 20px;
  display: block;
}
</style>
