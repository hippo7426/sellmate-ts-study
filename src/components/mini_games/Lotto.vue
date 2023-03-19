<template>
  <div>
    <h2>로또 번호 생성기</h2>
    <button @click="generateNumbers" :disabled="generating">생성</button>
    <div class="lotto-numbers-container">
      <div class="lotto-number" v-for="number in numbers" :key="number">
        {{ number }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { ref, type Ref } from 'vue'

export default defineComponent({
  setup() {
    const numbers: Ref<number[]> = ref([])
    const generating = ref(false);

    function generateNumbers() {
      numbers.value= [];
      generating.value = true;
      let lotoNumbers: number[] = []
      while (lotoNumbers.length < 7) {
        let randomNumber = Math.floor(Math.random() * 49) + 1
        if (!lotoNumbers.includes(randomNumber)) {
          lotoNumbers.push(randomNumber)
        }
      }
      lotoNumbers = lotoNumbers.sort((a, b) => a - b)
      let count = 0
      const intervalId = setInterval(() => {
        if (count === 7) {
          clearInterval(intervalId);
          generating.value = false;
          return
        }
        numbers.value[count] = lotoNumbers[count++]
      }, 500)
    }

    return {
      numbers,
      generating,
      generateNumbers,
    }
  },
})
</script>

<style lang="scss" scoped>
.lotto-numbers-container {
  display: flex;
  gap: 4px;
  margin-top: 10px;

  .lotto-number {
    width: 50px;
    height: 50px;
    border: 1px solid black;
    border-radius: 4px;

    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
