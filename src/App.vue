<template>
  <div>
    <TicTacToe />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Lotto from './components/mini_games/Lotto.vue';
import RockScissorPaper from './components/mini_games/RockScissorPaper.vue'
import TicTacToe from './components/mini_games/TicTacToe.vue';

export default defineComponent({
  components:{
    TicTacToe,
  },
  setup(){
    return {
      
    }
  }
})

</script>

<style scoped>

</style>