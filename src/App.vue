<template>
  <div>
    <RockScissorPaper />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Lotto from './components/mini_games/Lotto.vue';
import RockScissorPaper from './components/mini_games/RockScissorPaper.vue'
export default defineComponent({
  components:{
    RockScissorPaper
  },
  setup(){
    return {
      
    }
  }
})

</script>

<style scoped>

</style>