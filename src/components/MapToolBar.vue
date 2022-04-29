<template>

  <div class="MapToolBar-container">

    <div class="left-section">

      <div class="tool-bar">
        <canvas id="canvasSelectedTile" />
        <div v-if="!isFillActive" class="tool" @click="toggleFill">
          <i class="fas fa-fill-drip"/>
        </div>
        <div v-if="isFillActive" class="tool tool-selected" @click="toggleFill">
          <i class="fas fa-fill-drip"/>
        </div>

        <div class="tool" @click="changeRotation">{{rotation}}°</div>

      </div>
      <div>
        <SelectedTileOverlay v-model="selectedTile" />
        <Tileset v-model="selectedTile" :source="tileSet" @click="drawCanvasSelectedTile" />
      </div>

    </div>

    <div class="right-section">

      <Room v-if="map.data.length" :selectedTile="selectedTile" :rotation="rotation" :roomData="roomData" v-model="selectedCell" @updateRoomData="updateRoomData" @updateSelectedTile="updateSelectedTile"/>
      <canvas id="bufferTile"/>

    </div>
  </div>

</template>


<script>
import Tileset from "@/components/Tileset";
import Room from "@/components/Room";
import SelectedTileOverlay from "@/components/SelectedTile";

export default {
  name: "MapToolBar",

  inject: [
      'displayTileSize',
      'tileSetCols',
      'tileSet',
      'tileSize',
      'metadata',
      'level',
      'currentRoom',
      'roomRows',
      'roomCols',
      'mapRows',
      'mapCols'
  ],

  components: {
    Tileset,
    Room,
    SelectedTileOverlay,
  },

  props: {},

  emits: [],

  data() {
    return {
      isFillActive: false,
      selectedTile: {
        x: 0,
        y: 0,
        rotation: 0,
      },
      selectedCell: {
      },
      rotation: 0,
      currentTranslation: {},
      map: {
        metadata: [],
        data: []
      },
    }
  },

  provide() {
    return {}
  },

  computed: {
    displayTileSizePx(){
      return this.displayTileSize + 'px'
    },
    tileSetWidth(){
      return (this.displayTileSize * this.tileSetCols) + 'px'
    },
    roomData(){
      return this.map.data.filter(room => room.x === this.currentRoom.x && room.y === this.currentRoom.y)[0].roomData
    },
    translation(){
      switch (this.rotation) {
        case 0:
          return {
            x: 0,
            y: 0
          }
        case 90:
          return {
            x: this.displayTileSize / (this.displayTileSize / this.tileSize),
            y: 0
          }
        case 180:
          return {
            x: this.displayTileSize / (this.displayTileSize / this.tileSize),
            y: this.displayTileSize / (this.displayTileSize / this.tileSize)
          }
        case 270:
          return {
            x: 0,
            y: this.displayTileSize / (this.displayTileSize / this.tileSize)
          }
      }
    },
  },

  methods: {
    changeRotation(){
      this.rotation += 90
      if(this.rotation >= 360) this.rotation = 0
    },

    calculateTranslation(rotation){
      switch (rotation) {
        case 0:
          this.currentTranslation = {
            x: 0,
            y: 0
          }
          break;
        case 90:
          this.currentTranslation = {
            x: this.tileSize,
            y: 0
          }
          break;
        case 180:
          this.currentTranslation = {
            x: this.tileSize,
            y: this.tileSize
          }
          break;
        case 270:
          this.currentTranslation = {
            x: 0,
            y: this.tileSize
          }
      }
      this.drawCanvasSelectedTile()
    },

    updateSelectedTile(tile){
      this.selectedTile = tile
      this.rotation = tile.rotation
      this.drawCanvasSelectedTile()
    },

    updateRoomData(){
      if(!this.isFillActive){
        const cell = this.roomData.filter(cell => cell.x === this.selectedCell.x && cell.y === this.selectedCell.y)[0]
        cell.tile = this.selectedCell.tile
      } else {
        const cell = this.roomData.filter(cell => cell.x === this.selectedCell.x && cell.y === this.selectedCell.y)[0]
        let cells = this.roomData.filter(cells => cells.tile.x === cell.tile.x && cells.tile.y === cell.tile.y && cells.tile.rotation === cell.tile.rotation)
        cells.forEach(cell => {
          cell.tile = this.selectedCell.tile
        })
      }
    },

    updateRoomGraphics(){
      const tileSet = document.getElementById('tilesetImg')
      const c = document.getElementById('bufferTile')
      const ctx = c.getContext('2d')
      const buffer = document.getElementById('bufferTile')
      const b = document.getElementById('canvasRoom')
      const context = b.getContext('2d')

      c.width = this.tileSize
      c.height = this.tileSize

      b.width = this.roomCols * this.tileSize
      b.height = this.roomRows * this.tileSize

      for(let i = 0; i < this.roomData.length; i++){

        ctx.save()
          this.calculateTranslation(this.roomData[i].tile.rotation)
          ctx.translate(this.currentTranslation.x, this.currentTranslation.y)
          ctx.rotate(this.roomData[i].tile.rotation * Math.PI / 180)

          // Draw in buffer
          ctx.drawImage(
              tileSet,
              this.roomData[i].tile.x * this.tileSize,
              this.roomData[i].tile.y * this.tileSize,
              this.tileSize,
              this.tileSize,
              0,
              0,
              this.tileSize,
              this.tileSize
          )
        ctx.restore()

        // Draw in map
        context.drawImage(
            buffer,
            0,
            0,
            this.tileSize,
            this.tileSize,
            (this.roomData[i].x - 1) * this.tileSize,
            (this.roomData[i].y - 1) * this.tileSize,
            this.tileSize,
            this.tileSize
        )

        // Clear buffer
        ctx.clearRect(0, 0, this.tileSize, this.tileSize)
      }
      this.saveMap()

    },

    toggleFill(){
      this.isFillActive = !this.isFillActive
    },

    drawCanvasSelectedTile(){
      const tileSet = document.getElementById('tilesetImg')
      const c = document.getElementById('canvasSelectedTile')
      const ctx = c.getContext('2d')
      c.width = this.tileSize
      c.height = this.tileSize

      ctx.save()
      ctx.translate(this.translation.x, this.translation.y)
      ctx.rotate(this.rotation * Math.PI / 180)

      ctx.drawImage(
          tileSet,
          this.selectedTile.x * this.tileSize,
          this.selectedTile.y * this.tileSize,
          this.tileSize,
          this.tileSize,
          0,
          0,
          this.tileSize,
          this.tileSize
      )
      ctx.restore()
    },

    populateMap(){
      this.map.metadata.push(
          {
            level: this.level,
            tileset: this.tileSet
          }
      )
      for(let i = 1; i < this.mapRows + 1; i++){
        for(let j = 1; j < this.mapCols + 1; j++){

          let currentRoom = []
          for(let k = 1; k < this.roomRows + 1; k++){
            for(let l = 1; l < this.roomCols + 1; l++){
              currentRoom.push(
                  {
                    x: l,
                    y: k,
                    tile: {
                      x: 0,
                      y: 0,
                      rotation: 0
                    },
                  }
              )
            }
          }

          this.map.data.push(
              {
                x: j,
                y: i,
                roomData: currentRoom,
              }
          )
        }
      }

      this.createMap()

    },

    keyDown(e){
      // console.log(e.keyCode)
      if(e.keyCode === 82) this.changeRotation()
      if(e.keyCode === 70) this.toggleFill()
    },

    async fetchMap(){
      const res = await fetch('http://localhost:5000/map' + this.level)
      const data = await res.json()
      return data
    },

    async createMap(){
      const res = await fetch('http://localhost:5000/map' + this.level, {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(this.map)
      })
    },

    async saveMap(){
      const res = await fetch('http://localhost:5000/map' + this.level + `/${this.map.id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(this.map)
      })
    },
  },

  destroyed() {
    window.removeEventListener('keydown')
  },

  updated() {
    this.updateRoomGraphics()
  },

  mounted() {
    window.addEventListener('keydown', this.keyDown)
  },

  created() {
    this.fetchMap()
        .then(data => {
          this.map = {...data[0]}
          this.updateRoomGraphics()
          this.drawCanvasSelectedTile()
          if(!this.map.length) {
            this.map = {
              metadata: [],
              data: []
            }
            this.populateMap()
          }
        })
  },

}
</script>


<style scoped>
.MapToolBar-container {
  display: flex;
}
#canvasSelectedTile {
  width: v-bind(displayTileSizePx);
  height: v-bind(displayTileSizePx);
}
.tool-bar {
  width: v-bind(tileSetWidth);
  display: flex;
}
.tool {
  display: flex;
  align-items: center;
  justify-content: center;
  width: v-bind(displayTileSizePx);
  height: v-bind(displayTileSizePx);
}
.tool-selected {
  color: green;
}
button {
  padding: 5px 10px;
  border: none;
  background-color: hsl(200, 50%, 60%);
  border-radius: 3px;
  font-size: 1.2rem;
  color: white;
  cursor: pointer;
}
#bufferTile {
  /*opacity: 0;*/
}
</style>