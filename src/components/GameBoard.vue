<template>
  <div
    class="board"
    :class="{
      'line-x': winner === 'X',
      'line-o': winner === 'O',
    }"
  >
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
      @click="$emit('cell-click', index)"
    >
      {{ cell }}
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  board: Array,
  winner: String,
  winningPattern: Array,
})

defineEmits(['cell-click'])

const winningLineMap = {
  '0-1-2': 'row row-1',
  '3-4-5': 'row row-2',
  '6-7-8': 'row row-3',
  '0-3-6': 'column column-1',
  '1-4-7': 'column column-2',
  '2-5-8': 'column column-3',
  '0-4-8': 'diagonal diagonal-1',
  '2-4-6': 'diagonal diagonal-2',
}

const getWinningLineClass = () => {
  if (!props.winningPattern) return ''
  return winningLineMap[props.winningPattern.join('-')] || ''
}
</script>

<style scoped lang="scss">
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/animations';

.board {
  display: grid;
  grid-template-columns: repeat(3, 80px);
  width: 240px;
  margin: 0 auto;
  padding: 0;
  position: relative;
  background: transparent;


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
      top: calc(80px / 2 - #{$line-thickness} / 2);
    }

    &.row-2 {
      top: calc(80px * 1.5 - #{$line-thickness} / 2);
    }

    &.row-3 {
      top: calc(80px * 2.5 - #{$line-thickness} / 2);
    }

    &.column {
      width: $line-thickness;
      height: calc(100% - 24px);
      top: 12px;
    }

    &.column-1 {
      left: calc(80px / 2 - #{$line-thickness} / 2);
    }

    &.column-2 {
      left: calc(80px * 1.5 - #{$line-thickness} / 2);
    }

    &.column-3 {
      left: calc(80px * 2.5 - #{$line-thickness} / 2);
    }

    &.diagonal {
      width: 328px;
      height: $line-thickness;
      top: calc(50% - #{$line-thickness} / 2);
      left: calc(50% - 164px);
    }

    &.diagonal-1 {
      transform: rotate(45deg);
    }

    &.diagonal-2 {
      transform: rotate(-45deg);
    }
  }

  .cell {
    width: 80px;
    height: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2.2rem;
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

@media (min-width: 481px) {
  .board {
    grid-template-columns: repeat(3, 90px);
    width: 270px;

    .winning-line {
      &.row-1 {
        top: calc(90px / 2 - #{$line-thickness} / 2);
      }

      &.row-2 {
        top: calc(90px * 1.5 - #{$line-thickness} / 2);
      }

      &.row-3 {
        top: calc(90px * 2.5 - #{$line-thickness} / 2);
      }

      &.column-1 {
        left: calc(90px / 2 - #{$line-thickness} / 2);
      }

      &.column-2 {
        left: calc(90px * 1.5 - #{$line-thickness} / 2);
      }

      &.column-3 {
        left: calc(90px * 2.5 - #{$line-thickness} / 2);
      }

      &.diagonal {
        width: 370px;
        left: calc(50% - 185px);
      }
    }

    .cell {
      width: 90px;
      height: 90px;
      font-size: 2.6rem;
    }
  }
}

@media (min-width: 769px) {
  .board {
    grid-template-columns: repeat(3, 100px);
    width: 300px;

    .winning-line {
      &.row-1 {
        top: calc(100px / 2 - #{$line-thickness} / 2);
      }

      &.row-2 {
        top: calc(100px * 1.5 - #{$line-thickness} / 2);
      }

      &.row-3 {
        top: calc(100px * 2.5 - #{$line-thickness} / 2);
      }

      &.column-1 {
        left: calc(100px / 2 - #{$line-thickness} / 2);
      }

      &.column-2 {
        left: calc(100px * 1.5 - #{$line-thickness} / 2);
      }

      &.column-3 {
        left: calc(100px * 2.5 - #{$line-thickness} / 2);
      }

      &.diagonal {
        width: 410px;
        left: calc(50% - 205px);
      }
    }

    .cell {
      width: 100px;
      height: 100px;
      font-size: 3rem;
    }
  }
}
</style>