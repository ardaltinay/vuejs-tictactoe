<template>
  <div class="home">
    <h1>Tic-Tac-Toe</h1>
    <div>
      <label for="options">Choose game mode: </label>
      <select id="options" v-model="isSingleMode">
        <option>Single Player</option>
        <option>Multiplayer</option>
      </select>
    </div>
    <div v-if="isSingleMode == 'Single Player'">
      <label for="levels">Choose game level: </label>
      <select id="levels" v-model="gameDifficulty">
        <option>Easy</option>
        <option>Medium</option>
        <option>Hard</option>
      </select>
    </div>
    <game-component :gameDifficulty="gameDifficulty" :modeIs="isSingleMode"></game-component>
    <footer-component/>
  </div>
</template>

<script>
// @ is an alias to /src
import GameComponent from '@/components/GameComponent.vue'
import FooterComponent from '@/components/FooterComponent.vue'

export default {
  name: 'Home',
  components: {
    'game-component': GameComponent,
    'footer-component': FooterComponent
  },
  data() {
    return {
      isSingleMode: "",
      gameDifficulty: ""
    }
  },
  watch: {
    isSingleMode: function() {
      console.log("değişti");
    },
    gameDifficulty: function() {
      console.log("değişti-1");
    }
  }
}
</script>

<style>
  h1 {
    margin-top: 50px;
    margin-bottom: 50px;
  }
  div {
    margin-bottom: 20px;
  }
  select {
    width: 125px;
    background-color: #b3ffff;
    height: 30px;
    font-size: 16px;
    border: none;
    border-radius: 7px;
    cursor: pointer;
    color: #616161;
  }
  select:focus {
    outline: none;
  }
  select option:hover {
    background-color: blue;
    border: none;
  }
  label {
    font-size: 18px;
    color: #616161;
    font-weight: bold;
  }
</style>
