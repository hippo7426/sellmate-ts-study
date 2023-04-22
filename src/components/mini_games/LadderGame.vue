<template>
  <div>
    <h1>사다리 타기</h1>
    <div v-if="gameState === 'ready'">
      <div>
        <label
          >참가자<input v-model="playerInput" @keyup.enter="addPlayer" /></label
        ><button @click="addPlayer">추가</button>
      </div>
      <div>
        <label
          >옵션<input v-model="optionInput" @keyup.enter="addOption" /></label
        ><button @click="addOption">추가</button>
      </div>
      <div v-if="players.length">참가자 : {{ players }}</div>
      <div v-if="options.length">옵션 : {{ options }}</div>
      <button @click="generateLadder">사다리 만들기</button>
    </div>
    <div v-show="gameState === 'start'">
      <canvas ref="canvasRef" width="800" height="500"></canvas>
      <div>
        <button @click="resetGame">리셋</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
  name: 'LadderGame',
  setup() {
    const gameState = ref('ready')
    const canvasRef = ref<HTMLCanvasElement | null>(null)

    const players = ref<string[]>([])
    const options = ref<string[]>([])

    const playerInput = ref('')
    const optionInput = ref('')

    function addPlayer() {
      players.value.push(playerInput.value)
      playerInput.value = ''
    }

    function addOption() {
      options.value.push(optionInput.value)
      optionInput.value = ''
    }

    function generateLadder() {
      const ctx = canvasRef.value?.getContext('2d')
      if (!ctx) {
        alert('Canvas를 찾을 수 없습니다')
        return
      }
      gameState.value = 'start'

      const xPosList = Array.from({ length: players.value.length }, (v, k) => {
        return Math.floor(((k + 1) * 800) / (players.value.length + 1))
      })

      ctx.fillStyle = 'black'
      ctx.strokeStyle = 'black'

      xPosList.forEach((xPos, idx) => {
        ctx.beginPath()
        ctx.arc(xPos, 50, 5, 0, 2 * Math.PI)
        ctx.arc(xPos, 450, 5, 0, 2 * Math.PI)
        ctx.fill()

        ctx.beginPath()
        ctx.moveTo(xPos, 90)
        ctx.lineTo(xPos, 430)
        ctx.stroke()

        ctx.textAlign = 'center'
        ctx.fillText(players.value[idx], xPos, 70)
        ctx.fillText(options.value[idx], xPos, 470)
      })

      const yPosList: number[][] = []
      for (let i = 0; i < players.value.length - 1; i++) {
        const nthYPosList: number[] = []
        while (nthYPosList.length < 4) {
          const randomYPos = getRandomInteger(100, 400)
          if (
            nthYPosList.every(
              (yPos) =>
                !isPointInRange(randomYPos, { min: yPos - 15, max: yPos + 15 })
            )
          ) {
            if (
              i > 0 &&
              yPosList[i - 1].some((yPos) => {
                return isPointInRange(randomYPos, {
                  min: yPos - 30,
                  max: yPos + 30,
                })
              })
            ) {
              continue
            }
            nthYPosList.push(randomYPos)
          }
        }

        yPosList[i] = nthYPosList
      }

      yPosList.forEach((yPosSubList, areaIdx) => {
        yPosSubList.forEach((yPos) => {
          ctx.beginPath()
          ctx.moveTo(xPosList[areaIdx], yPos)
          ctx.lineTo(xPosList[areaIdx + 1], yPos)
          ctx.stroke()
        })
      })
    }

    function isPointInRange(
      point: number,
      range: { min: number; max: number }
    ) {
      return range.min <= point && point <= range.max
    }

    function getRandomInteger(min: number, max: number) {
      return Math.floor(Math.random() * (max - min + 1)) + min
    }

    function resetGame() {
      const ctx = canvasRef.value?.getContext('2d')
      ctx?.clearRect(0, 0, canvasRef.value!.width, canvasRef.value!.height)

      playerInput.value = ''
      optionInput.value = ''
      players.value = []
      options.value = []
      gameState.value = 'ready'
    }

    return {
      players,
      playerInput,
      options,
      optionInput,

      gameState,
      canvasRef,

      addPlayer,
      addOption,
      generateLadder,
      resetGame,
    }
  },
})
</script>

<style scoped lang="scss">
canvas {
  border: 1px solid #222222;
}
</style>
