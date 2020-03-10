<template>
  <transition-group
    v-if="myGame"
    tag="div"
    class="puzzle-grid"
    name="list-complete"
  >
    <span class="flex" v-for="(row, rowIndex) in currentGrid" :key="rowIndex">
      <transition-group
        tag="span"
        class="flex"
        name="list-complete"
        v-for="tile in row"
      >
        <TileEmpty
          v-if="tile === 0"
          :dimensionStyle="myGame.dimensionStyle"
          :key="tile+rowIndex"
        />
        <Tile
          v-else-if="isMovableTile(tile)"
          :number="tile"
          :showNumber="showNumbers"
          :backgroundStyle="buildBackground(tile)"
          :dimensionStyle="myGame.dimensionStyle"
          :clickable="true"
          @click.prevent.native="moveTile(tile)"
          :picture="picture"
          :key="tile+rowIndex"
        />
        <Tile
          v-else
          :number="tile"
          :showNumber="showNumbers"
          :backgroundStyle="buildBackground(tile)"
          :dimensionStyle="myGame.dimensionStyle"
          :clickable="false"
          :key="tile+rowIndex"
        />
      </transition-group>
    </span>
  </transition-group>
</template>

<script>
import { mapState, mapActions } from "vuex";
import Tile from "./Tile.vue";
import TileEmpty from "./TileEmpty.vue";

import { buildResponsiveBackground } from "../../../../core/src/picture";
import { isTileInMovableTiles } from "../../../../core/src/game";

export default {
  name: "Grid",
  components: {
    Tile,
    TileEmpty
  },
  props: {
    showNumbers: null,
    myGame: null,
    picture: null
  },
  computed: {
    currentGrid() {
      return this.myGame && this.myGame.currentGrid;
    },
    isVictory() {
      return this.myGame && this.myGame.isVictory;
    }
  },
  methods: {
    moveTile(tile) {
      this.$emit("moveTile", tile);
    },
    buildBackground(tile) {
      return buildResponsiveBackground(
        true,
        this.myGame.size,
        this.myGame.imageCoords[tile],
        this.myGame.tileSize
      );
    },
    isMovableTile(tile) {
      return !this.isVictory && isTileInMovableTiles(this.currentGrid, tile);
    }
  }
};
</script>

<style scoped>
.puzzle-grid {
  user-select: none;
  touch-action: manipulation;
}

.flex {
  align-items: center;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.list-complete-move {
  transition: transform .100s;
}
</style>
