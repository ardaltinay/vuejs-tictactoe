<template>
    <div class="game">
      <table v-if="gameBoard" cellspacing="0">
        <tr v-for="(row, rowIndex) in gameBoard" :key="rowIndex">
          <td v-for="(column, columnIndex) in gameBoard" :key="columnIndex" @click="userClick(rowIndex, columnIndex)">
            {{ gameBoard[rowIndex][columnIndex] }}
          </td>
        </tr>
      </table>
      <button class="restart-button" @click="restart">Restart</button>
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
      }
    },
    methods: {
      computerMove() {
        while(this.isAvailableForCompTurn) {
          let compRow = Math.floor(Math.random() * 3);
          let compColumn = Math.floor(Math.random() * 3);
          if(this.gameBoard[compRow][compColumn] === '') {
            this.gameBoard[compRow][compColumn] = 'O';
            break;
          } else {
            continue;
          }
        }
      },
      userClick(userRow, userColumn) {
        if(this.gameBoard[userRow][userColumn] === '') {
          this.gameBoard[userRow][userColumn] = 'X';
          this.$forceUpdate();
          if(this.isAvailableForCompTurn) {
            this.computerMove(); 
          }
        } else {
          return;
        }     
      },
      isAvailableForCompTurn() {
        for(let i=0; i<3; i++) {
          for(let j=0; j<3; j++) {
            if(this.gameBoard[i][j] !== '') {
              return false;
            } else {
              return true;
            }
          }
        }
      },
      restart() {
        this.gameBoard = [["","",""], ["","",""], ["","",""]];
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
    .game .restart-button:focus {
      outline: none;
    }
  </style>