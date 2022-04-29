<template>

  <div class="Room-container">

    <canvas id="canvasRoom"/>

    <div v-for="cell in roomData">
      <div
          v-if="!leftClick"
          class="cell"
          @mousedown="selectCell(cell.y, cell.x, $event)"
          :style="{'top': (cell.y - 1) * displayTileSize + 'px', 'left': (cell.x - 1) * displayTileSize + 'px'}"
      />
      <div
          v-if="leftClick"
          class="cell"
          @mouseenter="selectCell(cell.y, cell.x, $event)"
          :style="{'top': (cell.y - 1) * displayTileSize + 'px', 'left': (cell.x - 1) * displayTileSize + 'px'}"
      />
    </div>
  </div>

</template>


<script>
export default {
  name: "Room",

  inject: ['roomRows', 'roomCols', 'displayTileSize', 'metadata'],

  components: {},

  props: {
    selectedTile: Object,
    modelValue: Object,
    roomData: Array,
    rotation: Number,
  },

  emits: ['update:modelValue', 'updateRoomData', 'updateSelectedTile'],

  data() {
    return {
      leftClick: false,
      rightClick: false,
    }
  },

  provide() {
    return {}
  },

  computed: {
    displayTileSizePx(){
      return this.displayTileSize + 'px'
    },
    roomWidth(){
      return (this.roomCols * this.displayTileSize) + 'px'
    },
    roomHeight(){
      return (this.roomRows * this.displayTileSize) + 'px'
    },
  },

  methods: {
    mouseStateDown(e){
      if(e.button === 0) this.leftClick = true
      if(e.button === 2) this.rightClick = true
    },

    mouseStateUp(){
      this.leftClick = false
      this.rightClick = false
    },

    selectCell(y, x, e){
      if(this.leftClick){
        const metadata = this.metadata.filter(cell => cell.x === this.selectedTile.x && cell.y === this.selectedTile.y)[0]
        this.selectedCell = {
          x: x,
          y: y,
          tile: {
            x: metadata.x,
            y: metadata.y,
            rotation: this.rotation
          }
        }
        this.$emit('update:modelValue', this.selectedCell)
        this.$emit('updateRoomData')
      }

      if(e.button === 2){
        const cell = this.roomData.filter(cell => cell.x === x && cell.y === y)[0].tile
        this.$emit('updateSelectedTile', cell)
      }

    },

  },

  destroyed() {
    window.removeEventListener('mousedown')
    window.removeEventListener('mouseup')
    window.removeEventListener('contextmenu')
  },

  mounted() {
    window.addEventListener('mousedown', this.mouseStateDown)
    window.addEventListener('mouseup', this.mouseStateUp)
    document.addEventListener("contextmenu", function (e){
      e.preventDefault()
    }, false)
  },

  created() {
  },

}
</script>


<style scoped>
.Room-container {
  width: v-bind(roomWidth);
  height: v-bind(roomHeight);
  display: flex;
  flex-wrap: wrap;
  position: relative;
  background-color: hsl(105, 40%, 40%);
}
.cell {
  z-index: 2;
  position: absolute;
  border: 1px solid hsla(0, 0%, 0%, 0);
  width: v-bind(displayTileSizePx);
  height: v-bind(displayTileSizePx);
}
#canvasRoom {
  position: absolute;
  z-index: 1;
  width: v-bind(roomWidth);
  height: v-bind(roomHeight);
}
</style>