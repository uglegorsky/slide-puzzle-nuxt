<template>
  <div class="game">
    <transition-group name="game__map" tag="div" class="game__map">
      <template v-for="cellRow in cells">
        <cell v-for="cell in cellRow" :key="cell" :cellNumber="cell" />
      </template>
    </transition-group>
    <div class="status" v-show="isGameFinish()">Вы победитель!</div>
    <div class="game__info">
      <div class="game__info-wrapper">
        <div class="steps">Шагов: {{ steps }}</div>
        <timer :spendTime="spendTime" />
      </div>
      <button class="restart" @click="startGame">Играть заново</button>
    </div>
  </div>
</template>

<script>
import Cell from "@/components/Cell";
import Timer from "@/components/Timer";

export default {
  name: "Game",
  components: {
    cell: Cell,
    timer: Timer
  },
  data() {
    return {
      steps: 0,
      timeStart: null,
      timeEnd: null,
      spendTime: null,
      cells: [],
      axisX: null,
      axisY: null
    };
  },
  beforeMount() {
    document.addEventListener("keyup", this.handleKeyUp);
    this.startGame();
  },
  beforeDestroy() {
    document.removeEventListener("keyup", this.handleKeyUp);
  },
  methods: {
    calculateTimeGame() {
      this.timeStart = new Date().getTime();
      let timeInterval = setInterval(() => {
        this.timeEnd = new Date().getTime();
        this.spendTime = this.timeEnd - this.timeStart;
        if (this.isGameFinish()) clearInterval(timeInterval);
      }, 1000);
    },
    startGame() {
      this.steps = 0;
      let tableCells = [...Array(15).keys()].map(i => i + 1);
      this.shuffleArray(tableCells);
      tableCells.push(0);
      this.cells = this.chunkArray(tableCells, 4);
      this.axisX = 3;
      this.axisY = 3;
      this.calculateTimeGame();
    },
    handleKeyUp(event) {
      event.preventDefault();

      const keyUp = event.key;
      const [KEY_UP, KEY_DOWN, KEY_LEFT, KEY_RIGHT] = [
        "ArrowUp",
        "ArrowDown",
        "ArrowLeft",
        "ArrowRight"
      ];

      switch (true) {
        case keyUp === KEY_UP && this.axisY !== 3:
          this.updateGameField(
            this.axisY,
            this.axisX,
            ++this.axisY,
            this.axisX
          );
          break;
        case keyUp === KEY_DOWN && this.axisY !== 0:
          this.updateGameField(
            this.axisY,
            this.axisX,
            --this.axisY,
            this.axisX
          );
          break;
        case keyUp === KEY_LEFT && this.axisX !== 3:
          this.updateGameField(
            this.axisY,
            this.axisX,
            this.axisY,
            ++this.axisX
          );
          break;
        case keyUp === KEY_RIGHT && this.axisX !== 0:
          this.updateGameField(
            this.axisY,
            this.axisX,
            this.axisY,
            --this.axisX
          );
          break;
      }
      this.cells[this.axisY][this.axisX] = 0;
    },
    updateGameField(row, col, setRow, setCol) {
      this.cells[row][col] = this.cells[setRow][setCol];
      this.steps += 1;
    },
    isGameFinish() {
      const cellsGame = this.cells;
      const cellsUnchunk = cellsGame.flat(4);
      cellsUnchunk.splice(15, 1);
      const isCellsSort = cellsUnchunk.every(
        (element, index) => element == ++index
      );

      return isCellsSort;
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },
    chunkArray(array, chunk) {
      let i = null,
        j = null,
        chunkArray = [];
      for (i = 0, j = array.length; i < j; i += chunk) {
        chunkArray.push(array.slice(i, i + chunk));
      }
      return chunkArray;
    }
  }
};
</script>

<style lang="scss" scoped>
.game {
  &__map {
    padding: 10px;
    display: flex;
    flex-wrap: wrap;
    width: 500px;
    height: 500px;
    background-color: rgba(255, 255, 255, 0.6);
    border-radius: 10px;
    &-move {
      transition: 0.2s ease-in;
    }
  }
  &__info {
    display: flex;
    justify-content: space-between;
    margin-top: 2rem;
  }
}

.steps {
  font-size: 2rem;
}

.restart {
  border: none;
  outline: none;
  background: rgba($color: #000000, $alpha: 0.7);
  font-size: 2rem;
  padding: 1rem 3rem;
  color: #ffffff;
  border-radius: 10px;
  font-family: inherit;
  cursor: pointer;
  transition: all 0.3s ease-in;
  &:hover {
    color: #000000;
    background: rgba($color: #ffffff, $alpha: 0.7);
  }
}

.status {
  margin: 2rem 0;
  font-size: 3rem;
  text-transform: uppercase;
  color: #008000;
  text-align: center;
}
</style>
