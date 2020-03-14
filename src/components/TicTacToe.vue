<template>
  <div>
    <div class="container">
      <div class="heading-text">
        <h2>Tic-Tac-Toe</h2>
      </div>
      <div class="board">
        <div v-bind:key="i" v-for="(n, i) in 3">
          <div v-bind:key="j" v-for="(n, j) in 3">
            <square @click="performMove(i, j)" v-bind:value="squares[i][j]" />
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="board-footer">
        <div class="row">
          <div class="col-md-10">
            <p class="game-over-text">Current Player: {{currentPlayer}}</p>
          </div>
        </div>
        <div class="row" v-if="gameOver">
          <div class="col-md-5">
            <p class="game-over-text">{{ gameOverText }}</p>
          </div>
          <div class="col-md-5">
            <div class="text-right">
              <button class="btn btn-primary" @click="palyAgain">
                <span>Play again</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Square from "./Square";
export default {
  name: "TicTacToe",
  components: {
    Square
  },
  data() {
    return {
      gameOver: false,
      gameOverText: "",
      currentPlayer: "x",
      squares: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
      ]
    };
  },

  methods: {
    palyAgain() {
      this.gameOver = false;
      this.gameOverText = "";
      this.currentPlayer = "x";
      this.squares = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
      ];
    },
    performMove(x, y) {
      if (this.gameOver) {
        return;
      }

      if (!this.doMove(x, y, this.currentPlayer)) {
        return;
      }

      if (this.isGameOver()) {
        this.gameOver = true;
        this.gameOverText = this.calculateWinner(this.currentPlayer)
          ? `${this.currentPlayer} Win!`
          : "Draw";
        return;
      }

      this.currentPlayer = this.currentPlayer == "x" ? "o" : "x";
    },

    isGameOver() {
      return (
        this.getPossibleMoves().length === 0 ||
        this.calculateWinner("x") ||
        this.calculateWinner("o")
      );
    },

    getPossibleMoves() {
      let moves = [];
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          if (this.squares[i][j] === "") {
            moves.push({ x: i, y: j });
          }
        }
      }
      return moves;
    },

    doMove(x, y, player) {
      if (this.squares[x][y] !== "") {
        return false;
      }

      this.squares[x][y] = player;
      return true;
    },

    calculateWinner(player) {
      // |
      for (let j = 0; j < 3; j++) {
        if (
          this.squares[0][j] === player &&
          this.squares[1][j] === player &&
          this.squares[2][j] === player
        ) {
          return true;
        }
      }

      // -
      for (let i = 0; i < 3; i++) {
        if (
          this.squares[i][0] === player &&
          this.squares[i][1] === player &&
          this.squares[i][2] === player
        ) {
          return true;
        }
      }

      // *
      if (
        this.squares[0][0] === player &&
        this.squares[1][1] === player &&
        this.squares[2][2] === player
      ) {
        return true;
      }
      if (
        this.squares[2][0] === player &&
        this.squares[1][1] === player &&
        this.squares[0][2] === player
      ) {
        return true;
      }

      return false;
    }
  }
};
</script>
<style>
.board {
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  width: 304px;
  height: 304px;
}

.board-footer {
  margin: 0 auto;
  width: 304px;
}

.game-over-text {
  font-weight: bold;
  color: black;
  font-size: 16px;
  text-align: center;
  padding-top: 12px;
}

.heading-text {
  font-weight: bold;
  color: black;
  text-align: center;
  margin-right: 50px;
}
</style>
