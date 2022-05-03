<script setup>
import { ref, computed } from "vue";
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const calculateWinner = (squares) => {
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
const winner = computed(() => calculateWinner(board.value.flat()));
const makeMove = (x, y) => {
  if (winner.value) return;
  if (board.value[x][y] !== "") return;
  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};
const resetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<template>
  <main
    class="
      pt-8
      text-center
      font-poppins
      dark:bg-[#1B1B37]
      min-h-screen
      dark:text-[#F7F7F7]
    "
  >
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`border  border-[#FBF7F4] w-40 h-40 hover:bg-[#4A4A96] flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
    </div>
    <div>
      <h2 v-if="winner" class="text-6xl font-bold mb-8">
        Player "{{ winner }}" wins!
      </h2>
      <button
        @click="resetGame"
        class="
          px-4
          py-4
          bg-pink-500
          rounded
          uppercase
          font-bold
          hover:bg-pink-600
          duration-300
        "
      >
        Reset game
      </button>
    </div>
  </main>
</template>

<style>
</style>
