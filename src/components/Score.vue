<template>
  <div class="score-container">
    <div>
      <h2>Players Move: {{ player }}</h2>
      <div>
        <div class="red-score">Red: {{ redWins }} won.</div>
        <div class="blue-score">Blue: {{ blueWins }} won.</div>
      </div>
    </div>
  </div>
</template>

<script setup>

  import { defineProps } from 'vue';

  const props = defineProps({
    player: {
      type: String,
      required: true,
    },
    redWins: {
      type: Number,
      required: true,
    },
    blueWins: {
      type: Number,
      required: true,
    },
  });
</script>

<style scoped>
</style>