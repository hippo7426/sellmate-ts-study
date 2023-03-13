<template>
  <div>
    <WordChain />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import WordChain from './components/mini_games/WordChain.vue'

export default defineComponent({
  components:{
    WordChain,
  },
  setup(){
    return {
      
    }
  }
})

</script>

<style scoped>

</style>