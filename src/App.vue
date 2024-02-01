<script>
export default {
  data() {
    return {
      entries: new Array(9).fill(null),
      state: false,
      win: [],
      message: "X TURN",
    };
  },
  methods: {
    restart() {
      location.reload();
    },
    play(num) {
      if (!this.state) {
        this.entries[num] = "X";
      } else {
        this.entries[num] = "O";
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

      if (
        this.entries.length === 9 &&
        this.win.length === 0 &&
        this.entries.every((n) => n !== null)
      ) {
        this.message = `IT'S A TIE`;
      } else {
        this.message = `${this.state ? "O" : "X"} TURN`;
      }

      for (const combination of combinations) {
        const [a, b, c] = combination;

        if (
          this.entries[a] === this.entries[b] &&
          this.entries[b] === this.entries[c] &&
          this.entries[a] !== null &&
          this.entries[b] !== null &&
          this.entries[c] !== null
        ) {
          this.win = combination;
          this.message = `${this.entries[a]} WINS`;
        }
      }
    },
  },
};
</script>

<template>
  <main>
    <h4>
      {{ message }}
    </h4>
    <div class="game">
      <div v-for="row in 3" class="row">
        <button
          v-for="col in 3"
          class="tile"
          @click="play((row - 1) * 3 + (col - 1))"
          :disabled="
            entries[(row - 1) * 3 + (col - 1)] !== null || win.length !== 0
          "
          :style="{
            backgroundColor:
              win[row - 1] === (row - 1) * 3 + (col - 1) ||
              win[col - 1] === (row - 1) * 3 + (col - 1)
                ? 'rgb(110, 213, 110)'
                : '',
            color:
              win[row - 1] === (row - 1) * 3 + (col - 1) ||
              win[col - 1] === (row - 1) * 3 + (col - 1)
                ? 'white'
                : '',
          }"
        >
          {{ entries[(row - 1) * 3 + (col - 1)] || "\u00A0" }}
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
  background-color: rgb(255, 255, 255);
  border: 0;
}
.tile:disabled {
  color: rgb(57, 51, 51);
}
.restart {
  height: 2rem;
  font-size: 1rem;
  margin-top: 2rem;
}
</style>
