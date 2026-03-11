<template>
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
</template>

<script setup>

// props from App.vue
defineProps({
    winner: String,
    isDraw: Boolean,
    currentPlayer: String,
})
</script>

<style scoped lang="scss">
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/animations';

.status {
  font-size: 1rem;
  letter-spacing: 2px;
  line-height: 1.4;
  text-transform: uppercase;
  color: rgba(200, 245, 255, 0.7);
  margin-bottom: 16px;
  padding-left: 0;
  display: inline-block;
  animation: pulse-dim 1.4s ease-in-out infinite;

  &.winner {
    font-size: 1.25rem;
    letter-spacing: 3px;
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
    font-size: 1.15rem;
    letter-spacing: 3px;
    color: #fff;
    animation: draw-pulse 1.5s ease-in-out infinite;
    text-shadow:
      0 0 8px #fff,
      0 0 20px $neon,
      0 0 40px $neon;
  }
}

@media (min-width: 481px) {
  .status {
    font-size: 1.2rem;
    letter-spacing: 3px;
    margin-bottom: 20px;

    &.winner {
      font-size: 1.6rem;
      letter-spacing: 4px;
    }

    &.draw {
      font-size: 1.4rem;
      letter-spacing: 4px;
    }
  }
}

@media (min-width: 769px) {
  .status {
    font-size: 1.4rem;
    letter-spacing: 4px;
    margin-bottom: 20px;
    padding-left: 12px;

    &.winner {
      font-size: 2rem;
      letter-spacing: 6px;
    }

    &.draw {
      font-size: 1.7rem;
      letter-spacing: 6px;
    }
  }

}
</style>