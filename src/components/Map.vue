<template>

  <div class="Map-container">
    <div v-for="cellRow in mapRows" :key="cellRow">
      <div v-for="cellCol in mapCols" :key="cellCol">
        <div v-if="currentRoom.x !== cellRow || currentRoom.y !== cellCol" class="cell" @click="selectRoom(cellRow, cellCol)" />
        <div v-if="currentRoom.x === cellRow && currentRoom.y === cellCol" class="cell cell-selected" @click="selectRoom(cellRow, cellCol)" />
      </div>
    </div>
  </div>

</template>


<script>
export default {
  name: "Map",

  inject: ['roomCols', 'roomRows', 'mapCols', 'mapRows', 'currentRoom'],

  components: {},

  props: {
  },

  emits: [],

  data() {
    return {
      scale: 2,
    }
  },

  provide() {
    return {}
  },

  computed: {
    mapWidth(){
      return (this.roomCols * this.mapCols) * this.scale + 'px'
    },
    mapHeight(){
      return (this.roomRows * this.mapRows) * this.scale + 'px'
    },
    cellWidth(){
      return (this.roomCols) * this.scale + 'px'
    },
    cellHeight(){
      return (this.roomRows) * this.scale + 'px'
    },
  },

  methods: {
    selectRoom(x, y){
      this.currentRoom.x = x
      this.currentRoom.y = y
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
.Map-container {
  width: v-bind(mapWidth);
  height: v-bind(mapHeight);
  display: flex;
  flex-wrap: wrap;
}
.cell {
  width: v-bind(cellWidth);
  height: v-bind(cellHeight);
  outline: 1px solid hsla(0, 0%, 0%, .1);
  font-size: 8px;
}
.cell:hover {
  background-color: hsla(120, 50%, 80%, .5);
}
.cell-selected {
  background-color: hsla(220, 50%, 80%, .5);
}
</style>