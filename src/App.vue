<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen v-if="statusMatch === 'match'" :cardsContext="settings.cardsContext" @onFinish="onGetResult()" />
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'" />
  <copy-right />
</template>

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from '@/components/InteractScreen'
import ResultScreen from '@/components/ResultScreen'
import CopyRight from '@/components/CopyRight'
import {shuffled} from "./utils/array"

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight
  },
  data(){
    return {
      settings: {
        totalOfBlocks : 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    }
  },
  methods: {
    onHandleBeforeStart(config){
      console.log("running HandleBeforeStart ", config);
      // data ready 
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const fisrtCards =  Array.from({length: this.settings.totalOfBlocks / 2}, (_,i) => i+1);
      const secondCards =  [...fisrtCards]; // clone shadow
      const cards = [...fisrtCards, ...secondCards];
      
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards)))); // dao lon
      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "match";
    },

    onGetResult(){
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch to result component
      this.statusMatch = 'result';
    }
  },
};
</script>
