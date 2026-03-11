<template>
  <div class="app">
    <!-- Game title -->
    <h1>Tic Tac Toe</h1>

    <!-- Game status -->
    <p
      class="status"
      :class="{
        winner: winner,
        draw: isDraw,
        'winner-x': winner === 'X',
        'winner-o': winner === 'O',
      }"
    >
      <span v-if="winner">Winner: {{ winner }}</span>
      <span v-else-if="isDraw">It's a draw!</span>
      <span v-else>Current player: {{ currentPlayer }}</span>
    </p>

    <!-- Game board -->
    <div
      class="board"
      :class="{
        'line-x': winner === 'X',
        'line-o': winner === 'O',
      }"
    >
      <!-- Winning line -->
      <div
        v-if="winningPattern"
        class="winning-line"
        :class="getWinningLineClass()"
      ></div>

      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        :class="{
          x: cell === 'X',
          o: cell === 'O',
          highlight: winningPattern?.includes(index),
        }"
        @click="handleCellClick(index)"
      >
        {{ cell }}
      </div>
    </div>

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
                 
      <a href="https://www.linkedin.com/in/samuele-fiorini-38bba9325" target="_blank">
        <i class="fab fa-linkedin-in"></i>
      </a>
 
      <p> &copy;2026 Samuele Fiorini. All rights reserved.</p>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

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

// Return the correct winning line class
const getWinningLineClass = () => {
  if (!winningPattern.value) return ''

  const pattern = winningPattern.value.join('-')

  if (pattern === '0-1-2') return 'row row-1'
  if (pattern === '3-4-5') return 'row row-2'
  if (pattern === '6-7-8') return 'row row-3'

  if (pattern === '0-3-6') return 'column column-1'
  if (pattern === '1-4-7') return 'column column-2'
  if (pattern === '2-5-8') return 'column column-3'

  if (pattern === '0-4-8') return 'diagonal diagonal-1'
  if (pattern === '2-4-6') return 'diagonal diagonal-2'

  return ''
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
$neon: #00f5ff;
$neon-dim: rgba(0, 245, 255, 0.15);
$x-color: #e79950;
$o-color: #00f5ff;

$app-width: 300px;
$cell-size: 100px;
$line-thickness: 6px;

.app {
  text-align: center;
  font-family: 'Courier New', monospace;

  h1 {
    font-size: 3rem;
    letter-spacing: 10px;
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

  .status {
    font-size: 1.4rem;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: rgba(200, 245, 255, 0.7);
    margin-bottom: 20px;
    padding-left: 12px;
    display: inline-block;
    animation: pulse-dim 1.4s ease-in-out infinite;

    &.winner {
      font-size: 2rem;
      letter-spacing: 6px;
      color: #fff;
      animation: winner-glow 1.2s ease-in-out infinite alternate;
      text-shadow:
        0 0 10px #fff,
        0 0 20px $neon,
        0 0 40px $neon,
        0 0 80px $neon;
    }

    &.winner-x {
      color: $x-color;
      text-shadow:
        0 0 10px #fff,
        0 0 20px $x-color,
        0 0 40px $x-color,
        0 0 80px $x-color;
    }

    &.winner-o {
      color: $o-color;
      text-shadow:
        0 0 10px #fff,
        0 0 20px $o-color,
        0 0 40px $o-color,
        0 0 80px $o-color;
    }

    &.draw {
      font-size: 1.7rem;
      letter-spacing: 6px;
      color: #fff;
      animation: draw-pulse 1.5s ease-in-out infinite;
      text-shadow:
        0 0 8px #fff,
        0 0 20px $neon,
        0 0 40px $neon;
    }
  }

  .board {
    display: grid;
    grid-template-columns: repeat(3, $cell-size);
    width: $app-width;
    margin: 0 auto;
    padding: 0;
    position: relative;
    background: transparent;
    border: 2px solid $neon-dim;

    &.line-x .winning-line {
      background: $x-color;
      box-shadow:
        0 0 8px $x-color,
        0 0 18px $x-color,
        0 0 30px $x-color;
    }

    &.line-o .winning-line {
      background: $o-color;
      box-shadow:
        0 0 8px $o-color,
        0 0 18px $o-color,
        0 0 30px $o-color;
    }

    &::before {
      content: '';
      position: absolute;
      inset: 0;
      background:
        linear-gradient(rgba(0, 245, 255, 0.15), rgba(0, 245, 255, 0.15)) 33.33%
          0 / 1px 100%,
        linear-gradient(rgba(0, 245, 255, 0.15), rgba(0, 245, 255, 0.15)) 66.66%
          0 / 1px 100%,
        linear-gradient(rgba(0, 245, 255, 0.15), rgba(0, 245, 255, 0.15)) 0
          33.33% / 100% 1px,
        linear-gradient(rgba(0, 245, 255, 0.15), rgba(0, 245, 255, 0.15)) 0
          66.66% / 100% 1px;
      background-repeat: no-repeat;
      pointer-events: none;
      box-shadow: 0 0 30px rgba(0, 245, 255, 0.05);
      z-index: 1;
    }

    .winning-line {
      position: absolute;
      z-index: 3;
      border-radius: 999px;
      animation: line-grow 0.35s ease forwards;
      transform-origin: center center;

      &.row {
        width: calc(100% - 24px);
        height: $line-thickness;
        left: 12px;
      }

      &.row-1 {
        top: calc(#{$cell-size} / 2 - #{$line-thickness} / 2);
      }

      &.row-2 {
        top: calc(#{$cell-size} * 1.5 - #{$line-thickness} / 2);
      }

      &.row-3 {
        top: calc(#{$cell-size} * 2.5 - #{$line-thickness} / 2);
      }

      &.column {
        width: $line-thickness;
        height: calc(100% - 24px);
        top: 12px;
      }

      &.column-1 {
        left: calc(#{$cell-size} / 2 - #{$line-thickness} / 2);
      }

      &.column-2 {
        left: calc(#{$cell-size} * 1.5 - #{$line-thickness} / 2);
      }

      &.column-3 {
        left: calc(#{$cell-size} * 2.5 - #{$line-thickness} / 2);
      }

      &.diagonal {
        width: 410px;
        height: $line-thickness;
        top: calc(50% - #{$line-thickness} / 2);
        left: calc(50% - 205px);
      }

      &.diagonal-1 {
        transform: rotate(45deg);
      }

      &.diagonal-2 {
        transform: rotate(-45deg);
      }
    }

    .cell {
      width: $cell-size;
      height: $cell-size;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 3rem;
      font-weight: 700;
      letter-spacing: 0;
      background: transparent;
      border: 2px solid $neon-dim;
      cursor: pointer;
      transition:
        background 0.2s ease,
        transform 0.2s ease;
      position: relative;
      z-index: 2;

      &:hover {
        background: $neon-dim;
      }

      &.highlight {
        background: rgba(255, 255, 255, 0.03);
      }

      &.x {
        color: $x-color;
        animation: symbol-pop 0.25s ease;
        text-shadow:
          0 0 5px #fff,
          0 0 15px $x-color,
          0 0 30px $x-color,
          0 0 60px $x-color,
          0 0 100px $x-color;
      }

      &.o {
        color: $o-color;
        animation: symbol-pop 0.25s ease;
        text-shadow:
          0 0 5px #fff,
          0 0 15px $o-color,
          0 0 30px $o-color,
          0 0 60px $o-color,
          0 0 100px $o-color;
      }
    }
  }

  button {
    margin-top: 32px;
    padding: 12px 32px;
    font-family: 'Courier New', monospace;
    font-size: 0.8rem;
    letter-spacing: 4px;
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

.credits {
  margin-top: 40px;
  font-size: 1rem;
  color: rgba(200, 245, 255, 0.5);
  align-items: center;

  a {
    font-size: 2rem;
    color: rgba(200, 245, 255, 0.7);
    margin: 8px;

    &:hover {
      color: white;
      text-shadow:
        0 0 5px rgba(0, 245, 255, 0.5),
        0 0 15px rgba(0, 245, 255, 0.3),
        0 0 30px rgba(0, 245, 255, 0.2);
    }
  }

  p {
    margin-top: 2px;
    font-size: 0.8rem;
    color: rgba(200, 245, 255, 0.4);
  }
}

@keyframes symbol-pop {
  0% {
    opacity: 0;
    transform: scale(0.6);
  }

  70% {
    opacity: 1;
    transform: scale(1.1);
  }

  100% {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes line-grow {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@keyframes pulse-dim {
  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.4;
  }
}

@keyframes winner-glow {
  0% {
    transform: scale(1);
  }

  100% {
    transform: scale(1.05);
  }
}

@keyframes draw-pulse {
  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.6;
  }
}

@keyframes flicker {
  0%,
  19%,
  21%,
  23%,
  25%,
  54%,
  56%,
  100% {
    opacity: 1;
    text-shadow:
      0 0 5px #fff,
      0 0 15px $neon,
      0 0 30px $neon,
      0 0 60px $neon,
      0 0 100px $neon;
  }

  20%,
  24%,
  55% {
    opacity: 0.4;
    text-shadow: none;
  }
}
</style>