<template>
  <div class="app">
    <!-- Game title -->
    <h1>Tic Tac Toe</h1>

    <!-- Game status -->
    <GameStatus
      :winner="winner"
      :isDraw="isDraw"
      :currentPlayer="currentPlayer"
    />

    <!-- Game board -->
    <GameBoard
      :board="board"
      :winner="winner"
      :winningPattern="winningPattern"
      @cell-click="handleCellClick"
    />

    <!-- Reset button -->
    <button @click="resetGame">Reset Game</button>

    <!-- Credits -->
    <div class="credits">
      <a href="https://www.instagram.com/fiorini_sam_00" target="_blank">
        <i class="fab fa-instagram"></i>
      </a>

      <a href="https://github.com/SamFio00" target="_blank">
        <i class="fab fa-github"></i>
      </a>

      <a
        href="https://www.linkedin.com/in/samuele-fiorini-38bba9325"
        target="_blank"
      >
        <i class="fab fa-linkedin-in"></i>
      </a>

      <p>&copy;2026 Samuele Fiorini. All rights reserved.</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import GameStatus from './components/GameStatus.vue'
import GameBoard from './components/GameBoard.vue'

// Reactive game state
const board = ref([null, null, null, null, null, null, null, null, null])
const currentPlayer = ref('X')
const winner = ref(null)
const isDraw = ref(false)
const winningPattern = ref(null)

// Handle cell click
const handleCellClick = (index) => {
  if (winner.value || isDraw.value) return

  if (board.value[index] === null) {
    board.value[index] = currentPlayer.value
    checkWinner()

    if (!winner.value) {
      currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
    }
  }
}

// Check winner or draw
const checkWinner = () => {
  const winPatterns = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8], // rows
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8], // columns
    [0, 4, 8],
    [2, 4, 6], // diagonals
  ]

  for (const pattern of winPatterns) {
    const [a, b, c] = pattern

    if (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      winner.value = board.value[a]
      winningPattern.value = pattern
      return
    }
  }

  if (board.value.every((cell) => cell !== null)) {
    isDraw.value = true
  }
}

// Reset game state
const resetGame = () => {
  board.value = [null, null, null, null, null, null, null, null, null]
  currentPlayer.value = 'X'
  winner.value = null
  isDraw.value = false
  winningPattern.value = null
}
</script>

<style scoped lang="scss">
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/animations';

/* app */
.app {
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    gap: 20px;
    font-family: 'Courier New', monospace;
    padding: 24px 16px;

  h1 {
    font-size: 1.8rem;
    letter-spacing: 3px;
    line-height: 1.2;
    text-transform: uppercase;
    color: #fff;
    animation: flicker 4s infinite;
    text-shadow:
      0 0 5px #fff,
      0 0 15px $neon,
      0 0 30px $neon,
      0 0 60px $neon,
      0 0 100px $neon;
  }

  button {
    margin-top: 24px;
    padding: 10px 20px;
    font-family: 'Courier New', monospace;
    font-size: 0.7rem;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: rgb(200, 245, 255);
    background: transparent;
    border: 2px solid rgba(0, 245, 255, 0.2);
    cursor: pointer;
    transition: all 0.25s ease;

    &:hover {
      color: #fff;
      border-color: $neon;
      box-shadow:
        0 0 10px rgba(0, 245, 255, 0.3),
        inset 0 0 10px rgba(0, 245, 255, 0.05);
    }
  }
}

/* credits */
.credits {
  margin-top: 28px;
  font-size: 1rem;
  color: rgba(200, 245, 255, 0.5);
  align-items: center;

  a {
    font-size: 1.6rem;
    color: rgba(200, 245, 255, 0.7);
    margin: 6px;

    &:hover {
      color: white;
      text-shadow:
        0 0 5px rgba(0, 245, 255, 0.5),
        0 0 15px rgba(0, 245, 255, 0.3),
        0 0 30px rgba(0, 245, 255, 0.2);
    }
  }

  p {
    margin-top: 6px;
    font-size: 0.7rem;
    line-height: 1.5;
    color: rgba(200, 245, 255, 0.4);
  }
}

/* Responsive styles */

/* Responsive for tablets and larger screens */
@media (min-width: 481px) {
  .app {
    padding: 32px 20px;

    h1 {
      font-size: 2.4rem;
      letter-spacing: 6px;
    }

    button {
      margin-top: 26px;
      padding: 11px 24px;
      font-size: 1rem;
      letter-spacing: 3px;
    }
  }

  .credits {
    margin-top: 32px;

    a {
      font-size: 1.7rem;
      margin: 6px;
    }

    p {
      font-size: 0.75rem;
    }
  }
}

/* Responsive for desktop */
@media (min-width: 769px) {
  .app {
    padding: 0;

    h1 {
      font-size: 3rem;
      letter-spacing: 10px;
      line-height: normal;
    }

    button {
      margin-top: 32px;
      padding: 12px 32px;
      font-size: 1rem;
      letter-spacing: 4px;
    }
  }

  .credits {
    margin-top: 40px;
    font-size: 1rem;
    color: rgba(200, 245, 255, 0.5);
    align-items: center;

    a {
      font-size: 2rem;
      color: rgba(200, 245, 255, 0.7);
      margin: 8px;
    }

    p {
      margin-top: 2px;
      font-size: 0.8rem;
      color: rgba(200, 245, 255, 0.4);
    }
  }
}
</style>