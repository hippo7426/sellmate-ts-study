<template>
  <div class="word-chain-container">
    <section>
      <input
        id="input-1"
        v-model="inputModel[1]"
        :disabled="turn !== 1"
        @keydown.enter="onSubmit(1)"
      />
      <button @click="onSubmit(1)" :disabled="turn !== 1">확인</button>
    </section>
    <section>
      <input
        id="input-2"
        v-model="inputModel[2]"
        :disabled="turn !== 2"
        @keydown.enter="onSubmit(2)"
      />
      <button @click="onSubmit(2)" :disabled="turn !== 2">확인</button>
    </section>
  </div>
  <div style="text-align: center">
    <div>{{ turnMsg }}</div>
    <div>{{ endMsg }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent, nextTick, ref } from 'vue'

export default defineComponent({
  setup() {
    const NUM_OF_PLAYER = 2
    const inputModel = ref(['', '', ''])
    const turn = ref(1)
    const usedWords = new Set<string>()
    const turnMsg = ref('1번 플레이어의 차례입니다')
    const endMsg = ref('')

    function onSubmit(player: number) {
      const nextPlayer = (player % NUM_OF_PLAYER) + 1
      if (usedWords.has(inputModel.value[player])) {
        turnMsg.value = `${nextPlayer}번 플레이어의 승리입니다!`
        endMsg.value = Array.from(usedWords).join(',');
      } else {
        usedWords.add(inputModel.value[player])
        turn.value = nextPlayer
        inputModel.value[nextPlayer] = ''
        turnMsg.value = `${nextPlayer}번 플레이어의 차례입니다`

        const nextPlayerInput = document.getElementById(`input-${nextPlayer}`)

        if (nextPlayerInput) {
          nextTick(() => {
            nextPlayerInput.focus()
          })
        }
      }
    }

    return {
      inputModel,
      endMsg,
      turn,
      onSubmit,
      turnMsg,
    }
  },
})
</script>

<style lang="scss" scoped>
.word-chain-container {
  display: flex;
  justify-content: center;

  section {
    &:first-of-type {
      margin-right: 20px;
    }

    button {
      margin-left: 4px;
    }
  }
}
</style>
