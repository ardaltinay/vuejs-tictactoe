<template>
  <div class="game-single-player">
    <tic-tac-toe v-if="gameDifficulty" :difficulty="gameDifficulty"></tic-tac-toe>
  </div>
  
</template>

<script>
  import TicTacToe from '@/components/TicTacToe.vue'

  export default {
    name: 'SingleplayerComponent',
    components: {
      'tic-tac-toe': TicTacToe,
    },
    props: ['gameDifficulty']
    
  }
</script>

<style>

</style>