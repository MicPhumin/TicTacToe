<template>
  <main class="pt-8 text-center bg">
    <h1 class="mb-8 font-weight-bold">Tic Tac Toe</h1>
    <div class="px-16 mt-2">
      <v-btn-toggle
        v-model="mode"
        color="primary"
        mandatory
        class="d-flex flex-row mb-8"
      >
        <v-btn outlined class="flex-grow-1" value="3x3">
          <span class="text">3x3</span>
        </v-btn>
        <v-btn outlined class="flex-grow-1" value="4x4">
          <span class="text">4x4</span>
        </v-btn>
        <v-btn outlined class="flex-grow-1" value="5x5">
          <span class="text">5x5</span>
        </v-btn>
      </v-btn-toggle>
    </div>

    <!-- Start Tab 3x3 -->
    <template v-if="mode == '3x3'">
      <h3
        v-if="player === 'X'"
        class="mb-4 font-weight-medium close-mark-color"
      >
        Player {{ player }}'s turn
      </h3>
      <h3
        v-else-if="player === 'O'"
        class="mb-4 font-weight-medium circle-mark-color"
      >
        Player {{ player }}'s turn
      </h3>
      <div class="d-flex align-center board">
        <div v-for="(row, x) in board" :key="x" class="groupUp">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove(x, y)"
            :class="`cell material-icons-outlined 
          ${
            cell === 'X'
              ? 'close-mark-color'
              : cell === 'O'
              ? 'circle-mark-color'
              : 'dis-mark-color'
          }`"
          >
            {{
              cell === "X"
                ? "close"
                : cell === "O"
                ? "circle"
                : cell === "D"
                ? "delete"
                : ""
            }}
          </div>
        </div>
      </div>
      <h2 v-if="winner === 'X'" class="mb-4 font-weight-bold close-mark-color">
        Player ' {{ winner }} ' wins!
        <h3 class="mb-4 font-weight-bold">Scores : {{ score }}</h3>
      </h2>

      <h2
        v-else-if="winner === 'O'"
        class="mb-4 font-weight-bold circle-mark-color"
      >
        Player ' {{ winner }} ' wins!
        <h3 class="mb-4 font-weight-bold">Score : {{ score }}</h3>
      </h2>
      <h2 v-else-if="countClicks === 9" class="tex-6xl mb-4 font-weight-bold">
        Draw!
      </h2>
      <h2
        v-else-if="disablecell === 1 && countClicks === 8"
        class="tex-6xl mb-4 font-weight-bold"
      >
        Draw!
      </h2>
      <v-btn
        :disabled="disablecell === 1"
        @click="DisableCeller"
        class="mr-2"
        color="error"
        >Disable 1 Cell</v-btn
      >
      <v-btn @click="ResetGame" class="" color="primary">Reset Game</v-btn>

      <!-- <div class="test">
        <div class="a">1</div>
        <div class="b">2</div>
        <div class="c">3</div>
        <div class="d">4</div>
        <div class="e">5</div>
      </div> -->
    </template>
    <!-- End Tab 3x3 -->

    <!-- Start Tab 4x4 -->
    <template v-if="mode == '4x4'">
      <h3
        v-if="player4x4 === 'X'"
        class="text-xl mb-4 font-weight-medium close-mark-color"
      >
        Player {{ player4x4 }}'s turn
      </h3>
      <h3
        v-else-if="player4x4 === 'O'"
        class="text-xl mb-4 font-weight-medium circle-mark-color"
      >
        Player {{ player4x4 }}'s turn
      </h3>
      <div class="d-flex align-center board">
        <div v-for="(row, x) in board4x4" :key="x" class="groupUp">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove4x4(x, y)"
            :class="`cell material-icons-outlined 
          ${
            cell === 'X'
              ? 'close-mark-color'
              : cell === 'O'
              ? 'circle-mark-color'
              : 'dis-mark-color'
          }`"
          >
            {{
              cell === "X"
                ? "close"
                : cell === "O"
                ? "circle"
                : cell === "D"
                ? "delete"
                : ""
            }}
          </div>
        </div>
      </div>
      <h2
        v-if="winner4x4 === 'X'"
        class="tex-6xl mb-4 font-weight-bold close-mark-color"
      >
        Player ' {{ winner4x4 }} ' wins!
        <h3 class="mb-4 font-weight-bold">Scores : {{ score4x4 }}</h3>
      </h2>
      <h2
        v-else-if="winner4x4 === 'O'"
        class="tex-6xl mb-4 font-weight-bold circle-mark-color"
      >
        Player ' {{ winner4x4 }} ' wins!
        <h3 class="mb-4 font-weight-bold">Scores : {{ score4x4 }}</h3>
      </h2>
      <h2
        v-else-if="countClicks4x4 === 16"
        class="tex-6xl mb-4 font-weight-bold"
      >
        Draw!
      </h2>
      <h2
        v-else-if="disablecell4x4 === 1 && countClicks4x4 === 15"
        class="tex-6xl mb-4 font-weight-bold"
      >
        Draw!
      </h2>
      <v-btn
        :disabled="disablecell4x4 === 1"
        @click="DisableCeller4x4"
        class="mr-2"
        color="error"
        >Disable 1 Cell</v-btn
      >
      <v-btn @click="ResetGame4x4" class="" color="primary">Reset Game</v-btn>
    </template>
    <!-- End Tab 4x4 -->

    <!-- Start Tab 5x5 -->
    <template v-if="mode == '5x5'">
      <h3
        v-if="player5x5 === 'X'"
        class="text-xl mb-4 font-weight-medium close-mark-color"
      >
        Player {{ player5x5 }}'s turn
      </h3>
      <h3
        v-else-if="player5x5 === 'O'"
        class="text-xl mb-4 font-weight-medium circle-mark-color"
      >
        Player {{ player5x5 }}'s turn
      </h3>
      <div class="d-flex align-center board">
        <div v-for="(row, x) in board5x5" :key="x" class="groupUp">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove5x5(x, y)"
            :class="`cell material-icons-outlined 
          ${
            cell === 'X'
              ? 'close-mark-color'
              : cell === 'O'
              ? 'circle-mark-color'
              : 'dis-mark-color'
          }`"
          >
            {{
              cell === "X"
                ? "close"
                : cell === "O"
                ? "circle"
                : cell === "D"
                ? "delete"
                : ""
            }}
          </div>
        </div>
      </div>
      <h2
        v-if="winner5x5 === 'X'"
        class="tex-6xl mb-4 font-weight-bold close-mark-color"
      >
        Player ' {{ winner5x5 }} ' wins!
        <h3 class="mb-4 font-weight-bold">Scores : {{ score5x5 }}</h3>
      </h2>
      <h2
        v-else-if="winner5x5 === 'O'"
        class="tex-6xl mb-4 font-weight-bold circle-mark-color"
      >
        Player ' {{ winner5x5 }} ' wins!
        <h3 class="mb-4 font-weight-bold">Scores : {{ score5x5 }}</h3>
      </h2>
      <h2
        v-else-if="countClicks5x5 === 25"
        class="tex-6xl mb-4 font-weight-bold"
      >
        Draw!
      </h2>
      <h2
        v-else-if="disablecell5x5 === 1 && countClicks5x5 === 24"
        class="tex-6xl mb-4 font-weight-bold"
      >
        Draw!
      </h2>
      <v-btn
        :disabled="disablecell5x5 === 1"
        @click="DisableCeller5x5"
        class="mr-2"
        color="error"
        >Disable 1 Cell</v-btn
      >
      <v-btn @click="ResetGame5x5" class="" color="primary">Reset Game</v-btn>
    </template>
    <!-- End Tab 5x5 -->
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
const mode = ref("4x4");

// 3x3 start
const countClicks = ref(0);
const disablecell = ref(0);
const score = ref(0);
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};

const winner = computed(() => CalculateWinner(board.value.flat()));

const MakeMove = (x, y) => {
  if (winner.value) return;

  if (board.value[x][y] !== "") return;

  board.value[x][y] = player.value;

  player.value = player.value === "X" ? "O" : "X";
  countClicks.value++;
  console.log(countClicks.value);
  score.value = 9 - countClicks.value - disablecell.value;
};

const DisableCeller = () => {
  const i = Math.floor(Math.random() * board.value.length);
  const j = Math.floor(Math.random() * board.value[i].length);
  board.value[i][j] = "D";
  disablecell.value++;
};

const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
  countClicks.value = 0;
  disablecell.value = 0;
};
// 3x3 end

// 4x4 start
const countClicks4x4 = ref(0);
const disablecell4x4 = ref(0);
const score4x4 = ref(0);
const player4x4 = ref("X");
const board4x4 = ref([
  ["", "", "", ""],
  ["", "", "", ""],
  ["", "", "", ""],
  ["", "", "", ""],
]);
const CalculateWinner4x4 = (squares4) => {
  const lines4x4 = [
    [0, 1, 2, 3],
    [4, 5, 6, 7],
    [8, 9, 10, 11],
    [12, 13, 14, 15],
    [0, 4, 8, 12],
    [1, 5, 9, 13],
    [2, 6, 10, 14],
    [3, 7, 11, 15],
    [0, 5, 10, 15],
    [3, 6, 9, 12],
  ];
  for (let i = 0; i < lines4x4.length; i++) {
    const [a, b, c, d] = lines4x4[i];
    if (
      squares4[a] &&
      squares4[a] === squares4[b] &&
      squares4[a] === squares4[c] &&
      squares4[a] === squares4[d]
    ) {
      return squares4[a];
    }
  }
  return null;
};

const winner4x4 = computed(() => CalculateWinner4x4(board4x4.value.flat()));
const MakeMove4x4 = (x, y) => {
  if (winner4x4.value) return;

  if (board4x4.value[x][y] !== "") return;

  board4x4.value[x][y] = player4x4.value;

  player4x4.value = player4x4.value === "X" ? "O" : "X";
  countClicks4x4.value++;
  console.log(countClicks4x4.value);
  score4x4.value = 16 - countClicks4x4.value - disablecell4x4.value;
};

const DisableCeller4x4 = () => {
  const i = Math.floor(Math.random() * board4x4.value.length);
  const j = Math.floor(Math.random() * board4x4.value[i].length);
  board4x4.value[i][j] = "D";
  disablecell4x4.value++;
};
const ResetGame4x4 = () => {
  board4x4.value = [
    ["", "", "", ""],
    ["", "", "", ""],
    ["", "", "", ""],
    ["", "", "", ""],
  ];
  player4x4.value = "X";
  countClicks4x4.value = 0;
  disablecell4x4.value = 0;
};
// 4x4 End

// 5x5 start
const countClicks5x5 = ref(0);
const disablecell5x5 = ref(0);
const score5x5 = ref(0);
const player5x5 = ref("X");
const board5x5 = ref([
  ["", "", "", "", ""],
  ["", "", "", "", ""],
  ["", "", "", "", ""],
  ["", "", "", "", ""],
  ["", "", "", "", ""],
]);
const CalculateWinner5x5 = (squares5) => {
  const lines5x5 = [
    [0, 1, 2, 3, 4],
    [5, 6, 7, 8, 9],
    [10, 11, 12, 13, 14],
    [15, 16, 17, 18, 19],
    [20, 21, 22, 23, 24],
    [0, 5, 10, 15, 20],
    [1, 6, 11, 16, 21],
    [2, 7, 12, 17, 22],
    [3, 8, 13, 18, 23],
    [4, 9, 14, 19, 24],
    [0, 6, 12, 18, 24],
    [4, 8, 12, 16, 20],
  ];
  for (let i = 0; i < lines5x5.length; i++) {
    const [a, b, c, d, e] = lines5x5[i];
    if (
      squares5[a] &&
      squares5[a] === squares5[b] &&
      squares5[a] === squares5[c] &&
      squares5[a] === squares5[d] &&
      squares5[a] === squares5[e]
    ) {
      return squares5[a];
    }
  }
  return null;
};

const winner5x5 = computed(() => CalculateWinner5x5(board5x5.value.flat()));
const MakeMove5x5 = (x, y) => {
  if (winner5x5.value) return;

  if (board5x5.value[x][y] !== "") return;

  board5x5.value[x][y] = player5x5.value;

  player5x5.value = player5x5.value === "X" ? "O" : "X";
  countClicks5x5.value++;
  console.log(countClicks5x5.value);
  score5x5.value = 25 - countClicks5x5.value - disablecell5x5.value;
};

const DisableCeller5x5 = () => {
  const i = Math.floor(Math.random() * board5x5.value.length);
  const j = Math.floor(Math.random() * board5x5.value[i].length);
  board5x5.value[i][j] = "D";
  disablecell5x5.value++;
};

const ResetGame5x5 = () => {
  board5x5.value = [
    ["", "", "", "", ""],
    ["", "", "", "", ""],
    ["", "", "", "", ""],
    ["", "", "", "", ""],
    ["", "", "", "", ""],
  ];
  player5x5.value = "X";
  countClicks5x5.value = 0;
  disablecell5x5.value = 0;
};
// 5x5 End
</script>

<style>
.bg {
  background-color: #115e59;
  min-height: 100vh;
  color: white;
}

.close-mark-color {
  color: #06b6d4;
}

.dis-mark-color {
  color: #94a3b8;
}

.circle-mark-color {
  color: #f43f5e;
}

.board {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 8px;
}

.groupUp {
  display: flex;
}

.cell {
  border: 2px solid rgb(255, 255, 255);
  width: 80px;
  height: 80px;
  font-size: 42px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.cell:hover {
  background-color: #0f766e;
}

.a {
  display: flex;
  background-color: red;
  width: 80px;
  height: 80px;
  justify-content: center;
  align-items: center;
  margin: 40px;
}
.b {
  display: flex;
  background-color: blue;
  width: 80px;
  height: 80px;
  justify-content: center;
  align-items: center;
  margin: 40px;
}
.c {
  display: flex;
  background-color: yellow;
  width: 80px;
  height: 80px;
  justify-content: center;
  align-items: center;
  margin: 40px;
}
.d {
  display: flex;
  background-color: orange;
  width: 80px;
  height: 80px;
  justify-content: center;
  align-items: center;
  margin: 40px;
}
.e {
  display: flex;
  background-color: green;
  width: 80px;
  height: 80px;
  justify-content: center;
  align-items: center;
  margin: 40px;
}
.test {
  display: flex;
  justify-content: center;
  align-content: center;
  flex-direction: row-reverse;
  min-width: 100vh;
}
</style>