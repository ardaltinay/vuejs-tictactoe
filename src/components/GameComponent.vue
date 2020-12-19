<template>
  <div class="game-component">
    <tic-tac-toe :difficulty="gameDifficulty" :modeIs="modeIs" v-if="gameDifficulty != '' || modeIs == 'Multiplayer'"></tic-tac-toe>
  </div>
  
</template>

<script>
  import TicTacToe from '@/components/TicTacToe.vue'

  export default {
    name: 'GameComponent',
    components: {
      'tic-tac-toe': TicTacToe,
    },
    props: ['gameDifficulty', 'modeIs'],
  }
</script>

<style>

</style>