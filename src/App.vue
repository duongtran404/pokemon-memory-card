<template>
  <main-screen v-if="statusMatch === 'home'" @onStart="onHandle" />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'home'"
  />
  <p class="copyright">
    Copy by @<a href="https://github.com/duongtran404" target="_blank"
      >Dwong Tran</a
    >
  </p>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
      },
      statusMatch: "home",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  methods: {
    onHandle(value) {
      this.settings.totalOfBlocks = value.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];

      const cards = [...firstCards, ...secondCards];
      // console.log(cards);
      this.settings.cardsContext = shuffled(shuffled(cards));

      this.settings.startedAt = new Date().getTime();

      console.log(this.settings.cardsContext);

      this.statusMatch = "match";
    },

    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    },
  },
};
</script>
<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
  text-decoration: none;
}
</style>
