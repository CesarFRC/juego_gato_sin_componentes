<script>
import GameStatusVue from './GameStatus.vue';
import PlayersNamesVue from './PlayersNames.vue';
import ScoreBoardVue from './ScoreBoard.vue';
export default {
  data() {
    return {
      cells: Array(9).fill(""),
      currentPlayer: "X",
      playerXName: "",
      playerOName: "",
      playerXWins: 0,
      playerOWins: 0,
      winner: null,
      winningCombinations: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ],
      winners: [] // Nueva variable para almacenar los ganadores de cada partida
    };
  },
  methods: {
    handleCellClick(index) {
      if (!this.cells[index] && !this.winner) {
        this.cells[index] = this.currentPlayer;
        this.checkWinner();
        this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
      }
    },
    checkWinner() {
      for (let combination of this.winningCombinations) {
        const [a, b, c] = combination;
        if (
          this.cells[a] &&
          this.cells[a] === this.cells[b] &&
          this.cells[a] === this.cells[c]
        ) {
          this.winner = this.cells[a];
          this.winners.push(this.winner); // Agregar el ganador a la lista de ganadores
          if (this.winner === "X") {
            this.playerXWins++;
          } else {
            this.playerOWins++;
          }
          return;
        }
      }

      if (this.cells.every(cell => cell !== "")) {
        this.resetGame();
      }
    },
    resetGame() {
      this.cells = Array(9).fill("");
      this.currentPlayer = "X";
      this.winner = null;
    },
    getTotalGamesPlayed() {
      return this.playerXWins + this.playerOWins;
    },
    getCurrentPlayerName() {
      return this.currentPlayer === "X" ? this.playerXName : this.playerOName;
    }
  }
};
</script>
<template>
  <div class="game">
    <h1 style="font-size:400%">El Juego del Gato</h1>
    <div class="player-names">
      <div>
        Jugador X: <input type="text" v-model.lazy="playerXName" />
      </div>
      <div>
        Jugador O: <input type="text" v-model.lazy="playerOName" />
      </div>
    </div>
    <div class="score">Partidas ganadas de {{ playerXName }}:
      <div>{{ playerXWins }}</div>
    </div>
    <div class="score">Partidas ganadas de {{ playerOName }}:
      <div>{{ playerOWins }}</div>
    </div>
    <div class="game-stats">
      <table>
        <tr>
          <th>Partidas jugadas:</th>
          <td>{{ getTotalGamesPlayed() }}</td>
        </tr>
      </table>
    </div>
    <div class="game-stats">
      <table>
        <tr>
          <th>Partida</th>
          <th>Ganador</th>
        </tr>
        <tr v-for="(winner, index) in winners" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ winner }}</td>
        </tr>
      </table>
    </div>
    <div class="board-container">
      <div class="board">
        <div
          v-for="(cell, index) in cells"
          :key="index"
          @click="handleCellClick(index)"
          :class="{ cell: true, 'cell-selected': cell !== '' }"
        >
          {{ cell }}
        </div>
      </div>
      <div class="turn-bar">
        Turno del jugador: {{ getCurrentPlayerName() }}
      </div>
    </div>
    <div class="message" v-if="winner">
      ¡{{ winner }} ha ganado el juego!
    </div>
    <div class="button-container">
      <button @click="resetGame" :disabled="!winner">Reiniciar</button>
    </div>
  </div>
</template>



<style scoped>
.game {
  display: flex;
  flex-direction: column;
}

h1 {
  display: flex;
  justify-content: start;
}

.player-names {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.player-names > div {
  margin-right: 20px;
}

.score {
  display: flex;
  display: grid;
  grid-template-columns: auto auto;
  justify-content: space-between;
  background-color: tomato;
  padding: 10px;
  border: 5px solid black;
}

.game-stats {
  margin-top: 20px;
  text-align: center;
}

table {
  margin: 0 auto;
}

th,
td {
  padding: 5px 10px;
  border: 1px solid #ccc;
}

.board-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  grid-gap: 10px;
  width: 300px;
  height: 300px;
  background-color: #fff;
  border: 1px solid #333;
  padding: 10px;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #eee;
  border: 1px solid #ccc;
  font-size: 48px;
  cursor: pointer;
  transition: background-color 0.5s ease;
}

.cell-selected {
  background-color: #ff0000;
  color: #fff;
}

.cell:hover:not(.cell-selected) {
  background-color: #6a5acd;
}

.turn-bar {
  text-align: center;
  margin-top: 20px;
  font-size: 18px;
  color: #00ff00;
}

.message {
  text-align: center;
  margin-top: 20px;
  font-size: 24px;
  color: #00fa9a;
}

.button-container {
  text-align: center;
  margin-top: 20px;
}

button {
  display: flex;
  margin-left: 1000px;
  padding: 10px 20px;
  font-size: 18px;
  background-color: #555;
  color: #fff;
  border: none;
  cursor: pointer;
}

button:disabled {
  background-color: #ff0000;
  cursor: not-allowed;
}
</style>



