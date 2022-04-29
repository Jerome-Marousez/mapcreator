<template>
  <router-view/>

<!--  <div>{{map.metadata}}</div>-->
<!--  <div v-for="(room, id) in map.data" :key="id">-->
<!--    <div style="margin-bottom: 20px;">x: {{room.x}} / y: {{room.y}}</div>-->

<!--    <div v-for="(cell, id) in room.roomData" :key="id">-->
<!--      <div>{{cell}}</div>-->
<!--    </div>-->

<!--  </div>-->


</template>


<script>
export default {
  name: 'app',

  provide() {
    return {
      tileSize: 16,
      displayTileSize: 48,
      roomRows: this.roomRows,
      roomCols: this.roomCols,
      mapRows: this.mapRows,
      mapCols: this.mapCols,
      currentRoom: this.currentRoom,
      tileSetCols: this.tileSetCols,
      tileSetRows: this.tileSetRows,
      metadata: this.metadata,
      tileSet: this.tileSet,
      currentRoomTileSet: this.currentRoomTileSet,
      level: this.level,
    }
  },

  data(){
    return {
      roomRows: 4,
      roomCols: 4,
      mapRows: 2,
      mapCols: 2,
      metadata: [],
      tileSetCols: 4,
      tileSetRows: 16,
      level: 1,
      tileSet: "resources/tilesets/A.png",
      currentRoom: {
        x: 1,
        y: 1
      },
      currentRoomTileSet: [],
    }
  },

  computed: {
    tileSetGridSize(){
      return this.tileSetCols * this.tileSetRows
    }
  },

  methods:{

  },

  created(){
    let x = 0
    let y = 0
    for(let i = 0; i < this.tileSetGridSize; i++){
      if(x > 3) {
        x = 0
        y++
      }
      this.metadata.push({
        x: x,
        y: y,
        isWalkable: false,
        isDoor: false,
        interactive: 0,
      })
      x++
    }


  },

}
</script>

<style>
@import './styles/style.css';

</style>
