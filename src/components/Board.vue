<template>
  <div class="d-flex-center">
    <div class="card">
      <h1 class="large-font text-center">X</h1>

      <h1 class="text-center">{{ xCount }} Wins</h1>
      <div class="next-turn text-center" v-if="currentPlayer === 'X'">
        <h1>Your Turn</h1>
      </div>
    </div>
    <table>
      <tr v-for="r in 3" :key="r">
        <td
          v-text="board[r - 1][c - 1]"
          @click="turn(r - 1, c - 1)"
          :class="disable || board[r - 1][c - 1] !== '' ? 'click-event' : ''"
          v-for="c in 3"
          :key="c"
        ></td>
      </tr>
    </table>
    <div class="card">
      <h1 class="large-font text-center">O</h1>

      <h1 class="text-center">{{ yCount }} Wins</h1>
      <div class="next-turn text-center" v-if="currentPlayer === 'O'">
        <h1>Your Turn</h1>
      </div>
    </div>
  </div>

  <div v-if="winner" class="winner-card">
    <h1 class="text-center" v-if="winner !== 'Draw'">
      Player : {{ winner }} won the match
    </h1>
    <h1 class="text-center" v-else>Match Ended in a Draw</h1>
    <div class="full-width">
      <button class="button" type="button" @click="nextGame">Next Game</button>
      <button class="button" type="button" @click="completeReset">
        Reset Entire Game
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Board',
  data() {
    return {
      startPlayer: 'X',
      currentPlayer: 'X',
      xCount: 0,
      yCount: 0,
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
      winner: '',
      disable: false
    }
  },

  watch: {
    board: {
      handler: function () {
        const check = (x1, x2, y1, y2, z1, z2) => {
          return (
            this.board[x1][x2] !== '' &&
            this.board[y1][y2] !== '' &&
            this.board[z1][z2] !== '' &&
            this.board[x1][x2] === this.board[y1][y2] &&
            this.board[y1][y2] === this.board[z1][z2]
          )
        }
        const exists = (arr) => {
          return arr.some((row) => row.includes(''))
        }

        const conditionArray = [
          check(0, 0, 0, 1, 0, 2),
          check(1, 0, 1, 1, 1, 2),
          check(2, 0, 2, 1, 2, 2),
          check(0, 0, 1, 0, 2, 0),
          check(0, 1, 1, 1, 2, 1),
          check(0, 2, 1, 2, 2, 2),
          check(0, 0, 1, 1, 2, 2),
          check(0, 2, 1, 1, 2, 0)
        ]

        if (conditionArray.includes(true)) {
          this.winner = this.currentPlayer === 'X' ? 'O' : 'X'
          this.disable = true
          if (this.winner === 'X') this.xCount++
          else if (this.winner === 'O') this.yCount++
        } else if (!exists(this.board)) {
          this.winner = 'Draw'
        }
      },
      deep: true
    }
  },
  methods: {
    turn(x, y) {
      if (this.currentPlayer === 'X') {
        this.board[x][y] = this.currentPlayer
        this.currentPlayer = 'O'
      } else {
        this.board[x][y] = this.currentPlayer
        this.currentPlayer = 'X'
      }
    },
    nextGame() {
      this.disable = false
      this.board = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ]
      this.winner = ''
      if (this.startPlayer === 'X') {
        this.currentPlayer = 'O'
        this.startPlayer = 'O'
      } else {
        this.currentPlayer = 'X'
        this.startPlayer = 'X'
      }
    },
    completeReset() {
      this.disable = false
      this.board = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ]
      this.winner = ''
      this.xCount = 0
      this.yCount = 0
      this.currentPlayer = 'X'
      this.startPlayer = 'X'
    }
  }
}
</script>

<style scoped>
.d-flex-center {
  display: flex;
  justify-content: space-around;
}

.card {
  display: flex;
  flex-direction: column;

  min-width: 18vw;
  padding: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  border-radius: 5px; /* 5px rounded corners */
}

.large-font {
  font-size: 90px;
  margin-bottom: 0;
}

table {
  border-spacing: 10px;
  border-collapse: separate;
}

td {
  text-align: center;
  border: 2px solid white;
  padding: 30px;
  height: 70px;
  width: 70px;
  font-size: 60px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}

td:hover {
  transform: scale(1.025);
}

.click-event {
  pointer-events: none;
}

.next-turn {
  background: rgb(90, 90, 90, 0.4);
  margin-top: 20px;
  padding: 5px;
  box-shadow: 0 6px 12px 0 rgba(0, 0, 0, 0.2);
}

.large-font {
  font-size: 90px;
  margin-bottom: 0;
}

.winner-card {
  padding: 10px;
  margin-top: 20px;
  box-shadow: 0 6px 12px 0 rgba(0, 0, 0, 0.2);
  background: linear-gradient(
    90deg,
    rgba(25, 25, 25, 1) 0%,
    rgba(88, 91, 107, 1) 72%
  );
}

.full-width {
  width: 90vw;
  margin: auto;
  display: flex;
  justify-content: center;
}

.button {
  background-color: #29292d;
  cursor: pointer;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 0 40px 0 40px;
}

@media (max-width: 800px) {
  .card {
    order: 2;
  }
  .d-flex-center {
    flex-direction: column;
  }
  td {
    text-align: center;
    border: 2px solid white;
    padding: 10px;
    height: 50px;
    width: 50px;
    font-size: 30px;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
  }
  table {
    order: 1;
    margin: 40px;
  }
  .large-font {
    font-size: 30px;
    margin-bottom: 0;
  }
}
</style>