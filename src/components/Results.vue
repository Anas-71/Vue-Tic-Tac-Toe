<template>
  <div class="overview" :class="winner === 'X' ? 'blue' : 'red'">
    {{ winner === 'X' ? 'BLUE' : 'RED' }} WINS!
  </div>
</template>

<script setup>
  import { defineProps } from 'vue';

  const props = defineProps({
    winner: {
      type: String,
      required: true,
    }
  });
</script>

<style>
  .red {
    background-color: #F98756;
  }
  .blue {
    background-color: #4A8EF5;
  }
</style>