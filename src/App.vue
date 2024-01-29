<script>
export default {
  data() {
    return {
      symbol: new Array(9).fill(null),
      state: false,
      win: [],
    };
  },
  methods: {
    restart() {
      location.reload();
    },
    play(num) {
      if (!this.state) {
        this.symbol[num] = "X";
      } else {
        this.symbol[num] = "O";
      }
      this.state = !this.state;

      const combinations = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];

      for (const combination of combinations) {
        const [a, b, c] = combination;

        if (
          this.symbol[a] === this.symbol[b] &&
          this.symbol[b] === this.symbol[c] &&
          this.symbol[a] !== null &&
          this.symbol[b] !== null &&
          this.symbol[c] !== null
        ) {
          this.win = combination;
        }
      }

      if (
        this.symbol.length === 9 &&
        this.symbol.every((n) => n !== null && this.win === "")
      ) {
        this.win = null;
      }
    },
  },
};
</script>

<template>
  <main>
    <h1>TIC TAC TOE</h1>
    <h4>
      {{
        win.length === 0
          ? (state ? "O" : "X") + " turn!"
          : symbol[win[0]] + " wins!"
      }}
    </h4>
    <div class="game">
      <div v-for="row in 3" :key="row" class="row">
        <button
          v-for="col in 3"
          :key="col"
          class="tile"
          @click="play((row - 1) * 3 + (col - 1))"
          :disabled="
            symbol[(row - 1) * 3 + (col - 1)] !== null || win.length !== 0
          "
          :style="{
            color:
              win[row - 1] === (row - 1) * 3 + (col - 1) ||
              win[col - 1] === (row - 1) * 3 + (col - 1)
                ? 'green'
                : '',
          }"
        >
          {{ symbol[(row - 1) * 3 + (col - 1)] || "\u00A0" }}
        </button>
      </div>
    </div>
    <button class="restart" @click="restart()">Restart</button>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100svh;
  justify-content: center;
}
.game {
  background-color: black;
}
.row > :not(:first-child) {
  margin-left: 0.3rem;
}
.row:not(:first-of-type) > * {
  margin-top: 0.3rem;
}
.tile {
  width: 7rem;
  font-size: 5rem;
  background-color: white;
  border: 0;
}
.restart {
  height: 2rem;
  font-size: 1rem;
  margin-top: 2rem;
}
</style>
