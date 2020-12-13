<template>
  <div class="game-single-player">
    <EasyModeComponent v-if="gameDifficulty == 'Easy'"></EasyModeComponent>
    <MediumModeComponent v-else-if="gameDifficulty == 'Medium'"></MediumModeComponent>
    <HardModeComponent v-else-if="gameDifficulty == 'Hard'"></HardModeComponent>
  </div>
  
</template>

<script>
  import EasyModeComponent from '@/components/EasyModeComponent.vue'
  import MediumModeComponent from '@/components/MediumModeComponent.vue'
  import HardModeComponent from '@/components/HardModeComponent.vue'

  export default {
    name: 'SingleplayerComponent',
    components: {
      EasyModeComponent,
      MediumModeComponent,
      HardModeComponent
    },
    props: ['gameDifficulty']
    
  }
</script>

<style>

</style>