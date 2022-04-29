<template>

  <div class="SelectedTile-container">
    <div v-if="modelValue" id="selectedTile"/>
  </div>

</template>


<script>
export default {
  name: "SelectedTile",

  inject: ['displayTileSize', 'tileSetCols', 'tileSetRows'],

  components: {},

  props: {
    modelValue: Object,
  },

  emits: [],

  data() {
    return {}
  },

  provide() {
    return {}
  },

  computed: {
    tileSetWidth(){
      return (this.displayTileSize * this.tileSetCols) + 'px'
    },
    tileSetHeight(){
      return (this.displayTileSize * this.tileSetRows) + 'px'
    },
    tileSizePx(){
      return this.displayTileSize + 'px'
    },
    topOffset(){
      return (this.displayTileSize * this.modelValue.y) + 'px'
    },
    leftOffset(){
      return (this.displayTileSize * this.modelValue.x) + 'px'
    },
  },

  methods: {},

  destroyed() {
  },

  mounted() {
  },

  created() {
  },

}
</script>


<style scoped>
.SelectedTile-container {
  pointer-events: none;
  position: absolute;
  width: v-bind(tileSetWidth);
  height: v-bind(tileSetHeight);
  z-index: 4;
}

#selectedTile {
  width: v-bind(tileSizePx);
  height: v-bind(tileSizePx);
  top: v-bind(topOffset);
  left: v-bind(leftOffset);
  outline: 2px solid white;
  border-radius: 2px;
  position: absolute;
  z-index: 5;
}
</style>