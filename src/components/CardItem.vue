<template>
  <div :class="cardClasses" :style="cardStyles">
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card__face card__face--front">
        <div class="card__content"></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
    };
  },
  computed: {
    cardClasses() {
      return {
        card: true,
        disabled: this.isDisabled,
      };
    },
    cardStyles() {
      const height = (920 - 16 * 4) / Math.sqrt(this.cardsContext.length) - 16;
      const width = (height * 3) / 4;
      const perspective = width * 2;
      return {
        height: `${height}px`,
        width: `${width}px`,
        perspective: `${perspective}px`,
      };
    },
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;

      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style scoped lang="css">
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disabled .card__inner {
  cursor: default;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  background-size: contain;
  height: 100%;
  width: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>
