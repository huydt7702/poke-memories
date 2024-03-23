<template>
  <div class="screen">
    <div class="screen__inner" :style="cardStyles">
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="handleFlip"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardItem.vue";

const FLIP_BACK_DELAY = 800;
const GAME_FINISH_DELAY = 920;

export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      flippedCards: [],
    };
  },
  computed: {
    cardStyles() {
      const width =
        ((((920 - 16 * 4) / Math.sqrt(this.cardsContext.length) - 16) * 3) / 4 +
          16) *
        Math.sqrt(this.cardsContext.length);
      return {
        width: `${width}px`,
      };
    },
  },
  methods: {
    handleFlip(card) {
      if (this.isFlippedLimitReached()) return;
      this.addFlippedCard(card);

      if (this.isFlippedLimitReached()) {
        if (this.isMatched()) {
          this.handleMatched();
        } else {
          this.handleUnmatched();
        }
      }
    },
    isFlippedLimitReached() {
      return this.flippedCards.length === 2;
    },
    addFlippedCard(card) {
      this.flippedCards.push(card);
    },
    isMatched() {
      const [firstCard, secondCard] = this.flippedCards;
      return firstCard.value === secondCard.value;
    },
    handleMatched() {
      this.disableMatchedCards();
      this.resetFlippedCards();

      if (this.isGameFinished()) {
        this.finishGame();
      }
    },
    handleUnmatched() {
      setTimeout(() => {
        this.flipBackUnmatchedCards();
        this.resetFlippedCards();
      }, FLIP_BACK_DELAY);
    },
    disableMatchedCards() {
      this.flippedCards.forEach((card) => {
        const cardRef = this.$refs[`card-${card.index}`][0];
        cardRef.onEnabledDisableMode();
      });
    },
    resetFlippedCards() {
      this.flippedCards = [];
    },
    flipBackUnmatchedCards() {
      this.flippedCards.forEach((card) => {
        const cardRef = this.$refs[`card-${card.index}`][0];
        cardRef.onFlipBackCard();
      });
    },
    isGameFinished() {
      const cardsDisabledElement = document.querySelectorAll(
        ".screen .card.disabled"
      );
      if (!cardsDisabledElement) return;

      return cardsDisabledElement.length === this.cardsContext.length - 2;
    },
    finishGame() {
      setTimeout(() => {
        this.$emit("onFinish");
      }, GAME_FINISH_DELAY);
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
