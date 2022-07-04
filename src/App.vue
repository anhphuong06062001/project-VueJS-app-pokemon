<template>
  <MainScreenVue v-if="statusMatch === 'default'" @onStart="onHadleBeforeStart($event)"/>
  <InteractScreenVue 
  v-if="statusMatch === 'match'"
  :cardsContext="settings.cardsContext"
  @onFinish="onGetResult"
  />
  <ResultScreenVue v-if="statusMatch === 'result'" 
  :timer="timer" 
  @onStartAgain="statusMatch === 'default'"
  />

  <CoppyRightScreenVue />
</template>

<script>
import MainScreenVue from './components/MainScreen.vue';
import InteractScreenVue from './components/InteractScreen.vue';
import ResultScreenVue from './components/ResultScreen.vue';
import CoppyRightScreenVue from './components/CoppyRightScreen.vue';
import {shuffled} from "./utils/array"
export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    }
  },

  components: {
    MainScreenVue,
    InteractScreenVue,
    ResultScreenVue,
    CoppyRightScreenVue
  },

  methods: {
    onHadleBeforeStart(config){
      // console.log("running handle", config)
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        {length: this.settings.totalOfBlocks / 2}, 
        (_,i) => i+1);
      console.log(firstCards)

      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      console.log(cards)
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));
      this.settings.startedAt = new Date().getTime();
      //data redy
      this.statusMatch = "match";
    },

    onGetResult() {
      //get timer 
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch to result component
      this.statusMatch = "result";
    }
  }
};
</script>


