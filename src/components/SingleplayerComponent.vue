<template>
  <div class="game-single-player">
    <easy-mode-component v-if="gameDifficulty == 'Easy'"></easy-mode-component>
    <medium-mode-component v-else-if="gameDifficulty == 'Medium'"></medium-mode-component>
    <hard-mode-component v-else-if="gameDifficulty == 'Hard'"></hard-mode-component>
  </div>
  
</template>

<script>
  import EasyModeComponent from '@/components/EasyModeComponent.vue'
  import MediumModeComponent from '@/components/MediumModeComponent.vue'
  import HardModeComponent from '@/components/HardModeComponent.vue'

  export default {
    name: 'SingleplayerComponent',
    components: {
      'easy-mode-component': EasyModeComponent,
      'medium-mode-component': MediumModeComponent,
      'hard-mode-component': HardModeComponent
    },
    props: ['gameDifficulty']
    
  }
</script>

<style>

</style>