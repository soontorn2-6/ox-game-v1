<template>
  <div class="container">
    <h2 v-if="winner">Winner :{{ winner }}</h2>
    <h2>Player Move: {{ player }}</h2>
    <button @click="reset" class="btn btn-success mb-3">Reset</button>
    <div v-for="(_, x) in 3" :key="x" class="row">
      <button v-for="(_, y) in 3" :key="y" class="square" @click="move(x, y)">
        {{ squares[x][y] }}
      </button>
    </div>
    <h2 class="mt-5">History</h2>
    <div v-for="(game, idx) in history" :key="idx">
      Game {{ idx + 1 }}: {{ game }} won
    </div>
  </div>
</template>
<script>
import { computed, onMounted, ref, watch } from "vue";
let num_arr = 9;
const createSize = (size) => {
  let new_arr = [];
  // สร้าง arr หลัก ตามจำนวน input
  for (let i = 0; i < size; i++) {
    // console.log(i);
    new_arr.push([""]);
  }
  // สร้าง subarr  ตามจำนวน input
  for (let j = 0; j < size; j++) {
    // console.log(j);
    for (let k = 0; k < size - 1; k++) {
      new_arr[j].push("");
    }
  }
  return new_arr
};

const a = new createSize(num_arr)
console.log(a);

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    // console.log(a+' , '+b+' , '+c);
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      // console.log(squares[a]);
      return squares[a];
    }
  }
  return null;
};

export default {
  setup() {
    const player = ref("X");
    const squares = ref([
      ["", "", ""],
      ["", "", ""],
      ["", "", ""],
    ]);
    // console.log(squares);
    const winner = computed(() => calculateWinner(squares.value.flat()));
    // console.log(winner);
    const move = (x, y) => {
      console.log(x + "," + y);
      console.log(player.value);
      if (winner.value) return;

      squares.value[x][y] = player.value;

      player.value = player.value === "O" ? "X" : "O";
      console.log(player.value);
    };
    const reset = () => {
      player.value = "X";
      squares.value = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
    };
    const history = ref([]);
    watch(winner, (current, previous) => {
      console.log(winner);
      console.log(current + "  " + previous);
      if (current && !previous) {
        history.value.push(current);
        localStorage.setItem("history", JSON.stringify(history.value));
      }
      // JSON.stringify()
    });
    onMounted(() => {
      history.value = JSON.parse(localStorage.getItem(history)) ?? [];
    });

    return { winner, player, squares, move, reset, history };
  },
};
</script>
<style scoped>
.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 70px;
  font-weight: bold;
  line-height: 34px;
  height: 100px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 100px;
}
</style>