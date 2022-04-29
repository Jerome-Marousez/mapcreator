<template>

  <div class="tool-bar">

    <div @click="selectTool(0)">
      <div v-if="selectedTool === 'walkable'" class="tool tool-selected">
        <i class="fas fa-walking" />
      </div>
      <div v-if="selectedTool !== 'walkable'" class="tool">
        <i class="fas fa-walking" />
      </div>
    </div>

    <div @click="selectTool(1)">
      <div v-if="selectedTool === 'door'" class="tool tool-selected">
        <i class="fas fa-door-open" />
      </div>
      <div v-if="selectedTool !== 'door'" class="tool">
        <i class="fas fa-door-open" />
      </div>
    </div>

    <div @click="selectTool(2)">
      <div v-if="selectedTool === 'pin'" class="tool tool-selected">
        <i class="fas fa-map-pin" />
      </div>
      <div v-if="selectedTool !== 'pin'" class="tool">
        <i class="fas fa-map-pin" />
      </div>
    </div>

  </div>

  <div class="metadataOverlay-container" @click="selectTile">
    <div v-for="cell in metadata" :key="cell.id">

      <div v-if="selectedTool === 'walkable'" class="cell" @click="cell.isWalkable = !cell.isWalkable">
        <div v-if="cell.isWalkable">O</div>
        <div v-if="!cell.isWalkable">X</div>
      </div>

      <div v-if="selectedTool === 'door'" class="cell" @click="cell.isDoor = !cell.isDoor">
        <div v-if="cell.isDoor">O</div>
        <div v-if="!cell.isDoor">X</div>
      </div>

      <div v-if="selectedTool === 'pin'" class="cell" @click="selectPin(cell)">
        <div>{{cell.interactive}}</div>
      </div>

    </div>

  </div>

</template>


<script>
export default {
  name: "metadataOverlay",

  inject: ['displayTileSize', 'metadata', 'tileSetCols', 'tileSetRows'],

  components: {},

  props: {
  },

  emits: [],

  data() {
    return {
      selectedTool: 'walkable',
    }
  },


  computed: {
    tileSizePx(){
      return this.displayTileSize + 'px'
    },
    topOffset(){
      return (this.displayTileSize * this.selectedTile.y) + 'px'
    },
    leftOffset(){
      return (this.displayTileSize * this.selectedTile.x) + 'px'
    },
    tileSetWidth(){
      return (this.displayTileSize * this.tileSetCols) + 'px'
    },
    tileSetHeight(){
      return (this.displayTileSize * this.tileSetRows) + 'px'
    }
  },

  methods: {
    selectTile(e){
      this.selectedTile = {
        x: Math.floor(e.offsetX / this.displayTileSize),
        y: Math.floor(e.offsetY / this.displayTileSize)
      }
    },
    selectTool(id){
      const tools = ['walkable', 'door', 'pin']
      this.selectedTool = tools[id]
    },
    selectPin(cell){
      if(cell.interactive > 8){
        cell.interactive = 0
      } else {
        cell.interactive++
      }

    },
  },

  destroyed() {
  },

  mounted() {
  },

  created() {
  },

}
</script>


<style scoped>
.metadataOverlay-container {
  position: absolute;
  width: v-bind(tileSetWidth);
  height: v-bind(tileSetHeight);
  z-index: 3;
  display: flex;
  flex-wrap: wrap;
}
.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  width: v-bind(tileSizePx);
  height: v-bind(tileSizePx);
}
.tool-bar {
  width: v-bind(tileSetWidth);
  display: flex;
}
.tool {
  display: flex;
  justify-content: center;
  align-items: center;
  width: v-bind(tileSizePx);
  height: v-bind(tileSizePx);
}
.tool-selected {
  color: green;
}
</style>