<template>
  <main-screen v-if="isDefaultStatus" @onStart="handleBeforeStart" />
  <interact-screen
    v-if="isPlayingStatus"
    :cardsContext="settings.cardsContext"
    @onFinish="handleFinish"
  />
  <result-screen
    v-if="isFinishStatus"
    :timer="timer"
    @onStartAgain="resetGame"
  />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";

import { shuffled } from "./utils/array";

const GameStatus = {
  DEFAULT: "default",
  PLAYING: "playing",
  FINISH: "result",
};

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      gameStatus: GameStatus.DEFAULT,
      timer: 0,
    };
  },
  computed: {
    isDefaultStatus() {
      return this.gameStatus === GameStatus.DEFAULT;
    },
    isPlayingStatus() {
      return this.gameStatus === GameStatus.PLAYING;
    },
    isFinishStatus() {
      return this.gameStatus === GameStatus.FINISH;
    },
  },
  methods: {
    handleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;
      this.initializeCards();
      this.startTimer();

      this.gameStatus = GameStatus.PLAYING;
    },
    initializeCards() {
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, index) => index + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
    },
    startTimer() {
      this.settings.startedAt = Date.now();
    },
    stopTimer() {
      const currentTime = Date.now();
      this.timer = currentTime - this.settings.startedAt;
    },
    handleFinish() {
      this.stopTimer();
      this.gameStatus = GameStatus.FINISH;
    },
    resetGame() {
      this.gameStatus = GameStatus.DEFAULT;
      this.timer = 0;
    },
  },
};
</script>
