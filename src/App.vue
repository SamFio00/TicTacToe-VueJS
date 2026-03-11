<template>
  <div class="app">
    <h1>Tic Tac Toe</h1>
    <p v-if="winner">Winner: {{ winner }}</p>
    <p v-else-if="isDraw">It's a draw!</p>
    <p v-else>Current player: {{ currentPlayer }}</p>


    <div class="board">
      <div 
      @click="handleCellClick(index)" 
      class="cell"
      :class="{ 'x': cell === 'X', 'o': cell === 'O' }" 
      v-for="(cell, index) in board" 
      :key="index">
        {{ cell }}
      </div>
    </div>

    <button @click="resetGame">Reset Game</button>

  </div>
</template>

<script setup>
import { ref } from 'vue';

const board = ref([ null, null, null, null, null, null, null, null, null ]);
const currentPlayer = ref('X');
const winner = ref(null);
const isDraw = ref(false);

const handleCellClick = (index) => {
  if (winner.value || isDraw.value) return; 
  if (board.value[index] === null) {
    board.value[index] = currentPlayer.value;
    checkWinner();
    if (!winner.value) {
      currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
    }
  }
};

const checkWinner = () => {
  const winPatterns = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
    [0, 4, 8], [2, 4, 6]             // diagonals
  ];

  for (const pattern of winPatterns) {
    const [a, b, c] = pattern;
    if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
      winner.value = board.value[a];
      return;
    }
  }

  if (board.value.every(cell => cell !== null)) {
    isDraw.value = true;
  }
};

const resetGame = () => {
  board.value = [ null, null, null, null, null, null, null, null, null ];
  currentPlayer.value = 'X';
  winner.value = null;
  isDraw.value = false;
};


</script>

<style scoped lang="scss">
$neon: #00f5ff;
$neon-dim: rgba(0, 245, 255, 0.15);
$x-color: #8f2dff;
$o-color: #00f5ff;

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
      0 0 5px  #fff,
      0 0 15px $neon,
      0 0 30px $neon,
      0 0 60px $neon,
      0 0 100px $neon;

    @keyframes flicker {
      0%, 19%, 21%, 23%, 25%, 54%, 56%, 100% {
        text-shadow:
          0 0 5px  #fff,
          0 0 15px $neon,
          0 0 30px $neon,
          0 0 60px $neon,
          0 0 100px $neon;
        opacity: 1;
      }
      20%, 24%, 55% {
        text-shadow: none;
        opacity: 0.4;
      }
    }
  }

  p {
    font-size: 1.4rem;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: rgba(200, 245, 255, 0.7);
    margin-bottom: 20px;
    padding-left: 12px;
    display: inline-block;
    animation: pulse-dim 1.4s ease-in-out infinite;

    @keyframes pulse-dim {
      0%, 100% { opacity: 1; }
      50%       { opacity: 0.4; }
    }
  }

  .board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    padding: 0;
    background: transparent;
    position: relative;
    margin: 0 auto;
    width: 300px;

    &::before {
      content: '';
      position: absolute;
      inset: 0;
      background:
        linear-gradient(rgba(0,245,255,0.15), rgba(0,245,255,0.15)) 33.33% 0 / 1px 100%,
        linear-gradient(rgba(0,245,255,0.15), rgba(0,245,255,0.15)) 66.66% 0 / 1px 100%,
        linear-gradient(rgba(0,245,255,0.15), rgba(0,245,255,0.15)) 0 33.33% / 100% 1px,
        linear-gradient(rgba(0,245,255,0.15), rgba(0,245,255,0.15)) 0 66.66% / 100% 1px;
      background-repeat: no-repeat;
      pointer-events: none;
      box-shadow: 0 0 30px rgba(0,245,255,0.05);
    }

    .cell {
      width: 100px;
      height: 100px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 3rem;
      font-weight: 700;
      border: 2px solid $neon-dim;
      cursor: pointer;
      background: transparent;
      transition: background 0.2s ease;
      letter-spacing: 0;

      &:hover {
        background: $neon-dim;
      }

      &.x {
        color: $x-color;
        text-shadow:
          0 0 5px  #fff,
          0 0 15px $x-color,
          0 0 30px $x-color,
          0 0 60px $x-color,
          0 0 100px $x-color;
      }

      &.o {
        color: $o-color;
        text-shadow:
          0 0 5px  #fff,
          0 0 15px $o-color,
          0 0 30px $o-color,
          0 0 60px $o-color,
          0 0 100px $o-color;
      }
    }
  }

  button {
    margin-top: 32px;
    font-family: 'Courier New', monospace;
    font-size: 0.8rem;
    letter-spacing: 4px;
    text-transform: uppercase;
    background: transparent;
    color: rgb(200, 245, 255);
    border: 2px solid rgba(0, 245, 255, 0.2);
    padding: 12px 32px;
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
</style>