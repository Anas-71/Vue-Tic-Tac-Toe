<template>
  
  <div class="container" :class="player === 'X' ? 'red' : 'blue'">

    <Results v-if="winner" @click="reset" :winner="winner"></Results>
    
    <Score :player="player" :redWins="redWins" :blueWins="blueWins"></Score>

    <div class="ttt-container">
      <div v-for="n in 9" :key="n" @click="move(n - 1)" :class="player === 'X' ? 'red' : 'blue'">
        <img v-if="squares[n - 1]" :src="squares[n - 1] === 'X' ? xIcon : oIcon" alt="">
      </div>
    </div>

  </div>

</template>

<script setup>

  import xIcon from './assets/image/X.svg';
  import oIcon from './assets/image/O.svg';

  import Score from './components/Score.vue'
  import Results from './components/Results.vue'

  import { computed, onMounted, ref, watch } from 'vue'

  const player = ref('X');
  const squares = ref(['', '', '', '', '', '', '', '', '']);

  const winner = computed(() => calculateWinner(squares.value));

  const move = (x) => {
    if (winner.value) 
      return;
    if (squares.value[x] !== '')
      return;
    squares.value[x] = player.value;
    player.value = player.value === 'O' ? 'X' : 'O';
  };

  const reset = () => {
    player.value = 'X';
    squares.value = ['', '', '', '', '', '', '', '', ''];

  }

  const calculateWinner = squares => {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];
    console.log(squares);
    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
        return squares[a];
      }
    }
    if (squares.filter(value => value === '').length === 0)
      reset();
    
    return ''
  };

  const history = ref([]);
  watch(winner, (current, previous) => {
    if (current && !previous) {
      history.value.push(current);
      localStorage.setItem('history', JSON.stringify(history.value));
    }
  });

  const redWins = computed(() => {
    if (history.value.length === 0)
      return 0;
    return history.value.reduce((acc, v) => {
      return v === 'O' ? acc += 1 : acc;
    }, 0);
  });

  const blueWins = computed(() => {
    if (history.value.length === 0)
      return 0;
    return history.value.reduce((acc, v) => {
      return v === 'X' ? acc += 1 : acc;
    }, 0)
  });

  onMounted(() => {
    history.value = JSON.parse(localStorage.getItem('history')) ?? [];
  });

</script>

<style scoped>
  .blue {
    background-color: #4A8EF5;
  }
  .red {
    background-color: #F98756;
  }
</style>