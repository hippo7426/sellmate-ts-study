<template>
  <h1>Rock-Scissor-Paper!</h1>
  <template v-if="gameState === 'ready'">
    <div>
      <input v-model="newPlayerInput" @keydown.enter="addPlayer" />
      <button @click="addPlayer">Add Player</button>
    </div>
    <button @click="gameState = 'started'">Game Start</button>
  </template>
  <button v-if="gameState === 'started'" @click="playRound">Play Round</button>
  <button v-if="gameState === 'open'" @click="resetChoice">Reset Choice</button>
  <ul>
    <li v-for="player in players">
      <div>
        {{ player.name }}
        {{
          gameState !== 'open'
            ? (player.choice && 'Something') || ''
            : player.choice
        }}
      </div>
      <div v-if="gameState === 'started'">
        <button v-for="choice in choices" @click="player.choice = choice">
          {{ choice }}
        </button>
      </div>
    </li>
  </ul>
</template>

<script lang="ts">
import { ref, type Ref } from 'vue'

interface Player {
  name: string
  choice: Choice | null
}

type Choice = 'rock' | 'scissor' | 'paper'

export default {
  setup() {
    const newPlayerInput = ref('')
    const players: Ref<Player[]> = ref([])
    const gameState: Ref<'ready' | 'started' | 'open'> = ref('ready')
    const choices = ['rock', 'scissor', 'paper'] as const

    function addPlayer() {
      if (!newPlayerInput.value) return
      players.value.push({
        name: newPlayerInput.value,
        choice: null,
      })
      newPlayerInput.value = ''
    }

    function playRound() {
      let returnFlag = false
      const result: { [key in Choice]: Player[] } = {
        rock: [],
        scissor: [],
        paper: [],
      }

      players.value.forEach((player) => {
        if (!player.choice) {
          returnFlag = true
          return
        }
        result[player.choice].push(player)
      })

      if (returnFlag) {
        alert('선택을 완료하지 않은 플레이어가 있습니다!')
        return
      }

      const notChoosedCount = Object.values(result).reduce((acc, cur) => {
        const result = acc + (cur.length ? 0 : 1)
        return result
      }, 0)

      switch (notChoosedCount) {
        case 1: {
          judgeWinner(result)
          break
        }
        default: {
          gameState.value = 'open'
          alert('승자가 없습니다')
          break
        }
      }
    }

    function judgeWinner(result: { [key in Choice]: Player[] }) {
      const winnerList: string[] = []
      const validChoice = choices.filter((choice) => result[choice].length)

      gameState.value = 'open'

      players.value.forEach((player) => {
        if (
          validChoice.includes(player.choice!) &&
          player.choice === getWinner(validChoice[0], validChoice[1])
        ) {
          winnerList.push(player.name)
        }
      })
      alert(`승자: ${winnerList.join(',')}`)
    }

    function getWinner(pick1: Choice, pick2: Choice) {
      if (pick1 === pick2) {
        return null
      } else if (
        (pick1 === 'rock' && pick2 === 'scissor') ||
        (pick1 === 'paper' && pick2 === 'rock') ||
        (pick1 === 'scissor' && pick2 === 'paper')
      ) {
        return pick1
      } else {
        return pick2
      }
    }

    function resetChoice() {
      gameState.value = 'started'
      players.value.forEach((player) => {
        player.choice = null
      })
    }

    return {
      gameState,
      newPlayerInput,
      players,
      choices,

      addPlayer,
      playRound,
      resetChoice,
    }
  },
}
</script>
