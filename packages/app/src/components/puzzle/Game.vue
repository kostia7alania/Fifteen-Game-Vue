<template>
  <div class="board">
    <div v-if="myGame">
      <section class="game">
        <Grid
          :showNumbers="showNumbers"
          :myGame="myGame"
          @moveTile="moveTile"
        />
      </section>
      <section class="options">
        <div>
          <Checkbox
            @change="setShowNumbers"
            :checked="showNumbers"
            label="Show numbers"
          />
        </div>
      </section>
      <section class="infos">
        <Moves :turn="turn" />
        <Victory v-show="isVictory" :turn="turn" @create="create" />
      </section>
    </div>
  </div>
</template>

<script>
import Checkbox from "../ui/Checkbox.vue";
import Grid from "./Grid.vue";
import Victory from "./Victory.vue";
import Moves from "./Moves.vue";

import {
  DEFAULT_SIZE,
  associateTileToBackground,
  initGame,
  moveTile
} from "../../../../core/src";
import { buildResponsiveDimension } from "../../../../core/src/picture";

export default {
  name: "Game",
  components: {
    Grid,
    Victory,
    Moves,
    Checkbox
  },
  data() {
    return {
      showNumbers: false,
      myGame: null,
    };
  },
  computed: {
    turn() {
      return (this.myGame && this.myGame.turn) || -1;
    },
    isVictory() {
      return this.myGame && this.myGame.isVictory;
    }
  },
  methods: {
    setShowNumbers() {
      this.showNumbers = !this.showNumbers;
    },
    create() {
      initGame(DEFAULT_SIZE).then(newGame => {
        newGame.size = DEFAULT_SIZE;
        newGame.imageCoords = associateTileToBackground(newGame.resolvedGrid);
        const { dimensionStyle, tileSize } = buildResponsiveDimension(
          true,
          newGame.resolvedGrid.length
        );
        newGame.dimensionStyle = dimensionStyle;
        newGame.tileSize = tileSize;

        this.myGame = newGame;
      });
    },
    moveTile(tile) {
      const myGame = this.myGame;
      const newGame = Object.assign({}, myGame, {
        ...moveTile(myGame, tile)
      });
      this.myGame = newGame;
    }
  },
  created() {
    if (!this.myGame) {
      this.create();
    }
  }
};
</script>

<style scoped>
.board {
  display: flex;
  flex-direction: column;
}

section {
  margin: 10px 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.options {
  justify-content: flex-end;
}

.infos {
  flex-direction: column;
}

input {
  margin: 20px 10px 20px 30px;
}
</style>
