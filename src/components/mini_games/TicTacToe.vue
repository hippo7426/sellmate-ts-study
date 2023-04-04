<template>
  <div class="tic-tac-toe">
    <h1>Tic Tac Toe</h1>
    <div class="board">
      <div
        v-for="(cell, index) in cells"
        :key="index"
        :class="['cell', { highlight: winningCells?.includes(index) }, cell]"
        @click="play(index)"
      >
        {{ cell }}
      </div>
    </div>
    <button style="margin-top: 10px" v-if="isGameOver" @click="restart">
      재시작
    </button>
    <div v-else>
      <p>
        <b style="font-weight: 700">{{ isXTurn ? 'X' : 'O' }}</b> 의 차례
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent, computed, type Ref } from 'vue'

export default defineComponent({
  setup() {
    const cells: Ref<Array<'O' | 'X' | null>> = ref(Array(9).fill(null))
    const isXTurn = ref(true)
    const winningCells: Ref<readonly number[] | null> = ref(null)

    const isGameOver = computed(() => {
      return winningCells.value || !cells.value.includes(null)
    })

    function play(index: number) {
      if (cells.value[index] || isGameOver.value) {
        return
      }

      cells.value.splice(index, 1, isXTurn.value ? 'X' : 'O')
      isXTurn.value = !isXTurn.value

      const winner = calcWinner()

      if (winner) {
        winningCells.value = winner.cells
      }
    }

    function calcWinner() {
      const winCondition = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ] as const

      let winner = null

      winCondition.forEach((condition) => {
        const [cell1, cell2, cell3] = condition
        const lineContents = [
          cells.value[cell1],
          cells.value[cell2],
          cells.value[cell3],
        ]
        if (lineContents.every((content) => content === 'X')) {
          winner = { player: 'X', cells: condition }
        }
        if (lineContents.every((content) => content === 'O')) {
          winner = { player: 'O', cells: condition }
        }
      })

      return winner as unknown as {player: 'O'|'X', cells: readonly [number, number, number]}
    }

    function restart() {
      cells.value = Array(9).fill(null)
      isXTurn.value = true
      winningCells.value = null
    }

    return {
      cells,
      isXTurn,
      winningCells,
      isGameOver,
      play,
      restart,
    }
  },
})
</script>

<style lang="scss" scoped>
.tic-tac-toe {
  font-family: sans-serif;
  text-align: center;
  .board {
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
    max-width: 300px;
  }
  .cell {
    background-color: #e1e1e1;
    border: 1px solid #aaa;
    font-size: 48px;
    height: 100px;
    width: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;

    &:hover {
      background-color: #ddd;
    }
    &.highlight {
      &.X {
        background-color: #aaf;
      }

      &.O {
        background-color: #ee7160;
      }
    }
  }
}
</style>
