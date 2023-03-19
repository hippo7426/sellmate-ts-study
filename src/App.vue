<template>
  <div>
    <Lotto />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Lotto from './components/mini_games/Lotto.vue';

export default defineComponent({
  components:{
    Lotto
  },
  setup(){
    return {
      
    }
  }
})

</script>

<style scoped>

</style>