<template v-slot="static-content">
  <div class="game">
    <table v-if="gameBoard" cellspacing="0">
      <tr v-for="(row, rowIndex) in gameBoard" :key="rowIndex">
        <td :class="gameBoard[rowIndex][columnIndex] === 'X' ? 'td-X' : 'td-O'" v-for="(column, columnIndex) in gameBoard" 
          :key="columnIndex" @click="userClick(rowIndex, columnIndex)">
          <transition name="fade">
            <span v-if="gameBoard[rowIndex][columnIndex]">{{ gameBoard[rowIndex][columnIndex] }}</span>
          </transition>
        </td>
      </tr>
    </table>
    <transition name="fade"><p class="winner" v-if="winner">Game is over. The winner is {{ winner }}!</p></transition>
    <transition name="fade"><p class="winner" v-if="draw">Game is over. It's {{ draw }}!</p></transition>
    <button class="restart-button" @click="restart">Restart</button>
  </div>
</template>
  
<script>
export default {
  name: 'EasyModeComponent',
  data() {
    return {
      gameBoard: [
        ["","",""],
        ["","",""],
        ["","",""]
      ],
      winner: null,
      draw: null,               // mod seçildikten sonra değişirse oyun resetlenecek
      isUserTurn: true,         // multiplayer modu
      whichUser: 'X',
      filledNumber: 0
    }
  },
  props: ["difficulty", "modeIs"],
  methods: {
    easyMove() {
      for(;;) {
        const compRow = Math.floor(Math.random() * 3);
        const compColumn = Math.floor(Math.random() * 3);
        if(this.gameBoard[compRow][compColumn] === '') {
          this.gameBoard[compRow][compColumn] = 'O';
          break;
        } 
      }
    },
    mediumMove() {          
      for(let i=0; i<3; i++) {
        if(this.gameBoard[i][0] == 'X' && this.gameBoard[i][1] == 'X') {
          if(this.gameBoard[i][2] == '') {
            this.gameBoard[i][2] = 'O';
            return;
          }
        } else if(this.gameBoard[i][0] == 'X' && this.gameBoard[i][2] == 'X') {
          if(this.gameBoard[i][1] == '') {
            this.gameBoard[i][1] = 'O';
            return;
          }
        } else if(this.gameBoard[i][1] == 'X' && this.gameBoard[i][2] == 'X') {
          if(this.gameBoard[i][0] == '') {
            this.gameBoard[i][0] = 'O';
            return;
          }  
        } else if(this.gameBoard[0][i] == 'X' && this.gameBoard[1][i] == 'X') {
          if(this.gameBoard[2][i] == '') {
            this.gameBoard[2][i] = 'O';
            return;
          }        
        } else if(this.gameBoard[0][i] == 'X' && this.gameBoard[2][i] == 'X') {
          if(this.gameBoard[1][i] == '') {
            this.gameBoard[1][i] = 'O';
            return;
          }        
        } else if(this.gameBoard[1][i] == 'X' && this.gameBoard[2][i] == 'X') {
          if(this.gameBoard[0][i] == '') {
            this.gameBoard[0][i] = 'O';
            return;
          }        
        }
      }
      if(this.gameBoard[0][0] == 'X' && this.gameBoard[1][1] == 'X') {
        if(this.gameBoard[2][2] == '') {
          this.gameBoard[2][2] = 'O';
          return;
        } 
      } else if(this.gameBoard[0][0] == 'X' && this.gameBoard[2][2] == 'X') {
        if(this.gameBoard[1][1] == '') {
          this.gameBoard[1][1] = 'O';
          return;
        } 
      } else if(this.gameBoard[1][1] == 'X' && this.gameBoard[2][2] == 'X') {
        if(this.gameBoard[0][0] == '') {
          this.gameBoard[0][0] = 'O';
          return;
        } 
      } else if(this.gameBoard[0][2] == 'X' && this.gameBoard[1][1] == 'X') {
        if(this.gameBoard[2][0] == '') {
          this.gameBoard[2][0] = 'O';
          return;
        } 
      } else if(this.gameBoard[0][2] == 'X' && this.gameBoard[2][0] == 'X') {
        if(this.gameBoard[1][1] == '') {
          this.gameBoard[1][1] = 'O';
          return;
        } 
      } else if(this.gameBoard[1][1] == 'X' && this.gameBoard[2][0] == 'X') {
        if(this.gameBoard[0][2] == '') {
          this.gameBoard[0][2] = 'O';
          return;
        } 
      }
      this.easyMove();
    },
    hardMove() {            
      let playedNumber = this.playedMove();
      console.log(playedNumber);
      if(playedNumber == 1) {
        if(this.gameBoard[0][0] == 'X' || this.gameBoard[0][2] == 'X' || this.gameBoard[2][0] == 'X' || this.gameBoard[2][2] == 'X') {
          this.gameBoard[1][1] = 'O';
          return;
        } else if (this.gameBoard[1][1] == 'X') {
          this.gameBoard[0][0] = 'O';
          return;
        } else {
          if(this.gameBoard[0][0] == '') {
            this.gameBoard[0][0] = 'O';
            return;
          } else if(this.gameBoard[0][2] == '') {
            this.gameBoard[0][2] = 'O';
            return;
          } else if(this.gameBoard[2][0] == '') {
            this.gameBoard[2][0] = 'O';
            return;
          } else if(this.gameBoard[2][2] == '') {
            this.gameBoard[2][2] = 'O';
            return;
          }
        }
      } else if(playedNumber == 3) {
        if(this.gameBoard[1][1] == '') {
          this.gameBoard[1][1] = 'O';
          return;
        }
      }
      this.mediumMove();
    },
    computerMove() {
      if(this.difficulty == 'Easy') {
        this.easyMove();
      } else if(this.difficulty == 'Medium') {
        this.mediumMove();
      } else if(this.difficulty == 'Hard') {
        this.hardMove();
      }
    },
    userClick(userRow, userColumn) {
      if(this.winner || !this.isUserTurn) {
        return;
      }
      if(this.gameBoard[userRow][userColumn] === '' && this.modeIs != 'Multiplayer') {
        this.isUserTurn = false;
        this.gameBoard[userRow][userColumn] = 'X';
        if(this.isDone()) {
          this.winner = 'X';
        } else if(this.isDraw()) {
          this.draw = 'draw';
        } else {
          setTimeout(() => {
            this.computerMove();
            if(this.isDone()) {               
              this.winner = 'O';              
              }
            this.isUserTurn = true;
            this.$forceUpdate();
          }, 800); 
        }
        this.$forceUpdate();
      }  else {
        return;
      }
    },
    playedMove() {
      this.filledNumber = 0;
      for(let i = 0; i < 3; i++) {
        for(let j = 0; j < 3; j++) {
          if(this.gameBoard[i][j] != '') {
            this.filledNumber++;
          }
        }
      }
      return this.filledNumber;
    },
    isDone() {
      for(let i=0; i<3; i++) {
        if(this.gameBoard[0][i] && this.gameBoard[0][i] === this.gameBoard[1][i] && this.gameBoard[0][i] === this.gameBoard[2][i]) {
          return true;
        }
      }
      for(let i=0; i<3; i++) {
        if(this.gameBoard[i][0] && this.gameBoard[i][0] === this.gameBoard[i][1] && this.gameBoard[i][0] === this.gameBoard[i][2]) {
          return true;
        }
      }
      if(this.gameBoard[0][0] && this.gameBoard[0][0] === this.gameBoard[1][1] && this.gameBoard[0][0] === this.gameBoard[2][2]) {
        return true;
      }
      if(this.gameBoard[0][2] && this.gameBoard[0][2] === this.gameBoard[1][1] && this.gameBoard[0][2] === this.gameBoard[2][0]) {
        return true;
      }
      return false;
    },
    isDraw() {
      let emptyCount = 0;
      for(let i=0; i<3; i++) {
        for(let j=0; j<3; j++) {
          if(this.gameBoard[i][j] === '') {
            emptyCount++;
          }
        }
      }
      if(!emptyCount) {
        return true;
      }
      return false;
    },
    restart() {
      this.gameBoard = [["","",""], ["","",""], ["","",""]];
      this.winner = null;
      this.draw = null;
      this.isUserTurn = true;
      this.filledNumber = 0;
    }     
  }
}
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .game table {
    margin: 65px auto 50px;
  }
  .game table tr td {
    width: 70px;
    height: 70px;
    font-size: 26px;
    line-height: 70px;
  }
  .game table tr .td-X {
    color: #c13a3a;
  }
  .game table tr .td-O {
    color: #2b2b2b;
  }
  .game table tr td:nth-child(1),
  .game table tr td:nth-child(2) {
    border-right: 2px solid #070e7c;
  }
  .game table tr:nth-child(1) td,
  .game table tr:nth-child(2) td {
    border-bottom: 2px solid #070e7c;
  }
  .game .winner {
    font-size: 21px;
    color: #00648a;
  }
  .game .restart-button {
    border: 1px solid #cccccc;
    border-radius: 10px;
    background-color: inherit;
    padding: 10px 20px;
    cursor: pointer;
    margin-top: 15px;
  }
  .game .restart-button:hover {
    background-color: #b3ffff;
  }
  .game .restart-button:focus {
    outline: none;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
</style>