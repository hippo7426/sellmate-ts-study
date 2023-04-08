<template>
  <div>
    <CardMatching />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import CardMatching from './components/mini_games/CardMatching.vue'

export default defineComponent({
  components:{
    CardMatching
  },
  setup(){
    return {
      
    }
  }
})

</script>

<style scoped>

</style>