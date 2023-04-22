<template>
  <div>
    <LadderGame />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import LadderGame from './components/mini_games/LadderGame.vue';

export default defineComponent({
  components:{
    LadderGame
  },
  setup(){
    return {
      
    }
  }
})

</script>

<style scoped>

</style>