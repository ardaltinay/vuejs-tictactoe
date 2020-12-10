<template>
    <div class="game">
      <table v-if="gameBoard" cellspacing="0">
        <tr v-for="(row, rowIndex) in gameBoard" :key="rowIndex">
          <td v-for="(column, columnIndex) in gameBoard" :key="columnIndex" @click="userClick(rowIndex, columnIndex)">
            {{ gameBoard[rowIndex][columnIndex] }}
          </td>
        </tr>
      </table>
      <button class="restart-button">Restart</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Main',
    data() {
      return {
        gameBoard: [
          ["","",""],
          ["","",""],
          ["","",""]
        ],
        isAvailable: false
      }
    },
    methods: {
      computerMove() {
        this.isAvailable = true;
        while(this.isAvailable) {
          let compRow = Math.floor(Math.random() * 3);
          let compColumn = Math.floor(Math.random() * 3);
          if(this.gameBoard[compRow][compColumn]) {
            if(this.gameBoard[compRow][compColumn] === '') {
              this.gameBoard[compRow][compColumn] = 'O';
              this.isAvailable = false;
            } else if(this.gameBoard[compRow][compColumn] === 'X' || this.gameBoard[compRow][compColumn] === 'O') {
              continue;
            }
          } else {
            this.isAvailable = false;
          }
        }
      },
      userClick(userRow, userColumn) {
        if(this.gameBoard[userRow][userColumn] === 'O' || this.gameBoard[userRow][userColumn] === 'X') {
          return;
        } else {
          this.gameBoard[userRow][userColumn] = 'X';
          this.computerMove();
          this.$forceUpdate();
        }       
      }     
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
    .game table {
      margin: 50px auto;
    }
    .game table tr td {
      width: 50px;
      height: 50px;
      font-size: 20px;
      line-height: 50px;
    }
    .game table tr td:nth-child(1),
    .game table tr td:nth-child(2) {
      border-right: 2px solid #070e7c;
    }
    .game table tr:nth-child(1) td,
    .game table tr:nth-child(2) td {
      border-bottom: 2px solid #070e7c;
    }
    .game .restart-button {
      border: 1px solid #cccccc;
      border-radius: 10px;
      background-color: aqua;
      padding: 10px;
      cursor: pointer;
    }
  </style>