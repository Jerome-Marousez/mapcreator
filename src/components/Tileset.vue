<template>

  <div class="Tileset-container">
    <img :src="source" id="tilesetImg" @click="selectTile">
  </div>

</template>


<script>
export default {
  name: "Tileset",

  inject: ['displayTileSize', 'tileSetCols', 'tileSetRows'],

  components: {},

  props: {
    source: String,
    modelValue: Object,
  },

  emits: ['update:modelValue'],

  data() {
    return {

    }
  },

  computed: {
    tileSetWidth(){
      return (this.displayTileSize * this.tileSetCols) + 'px'
    },
    tileSetHeight(){
      return (this.displayTileSize * this.tileSetRows) + 'px'
    }
  },

  methods: {
    selectTile(e){
      const selectedTile = {
        x: Math.floor(e.offsetX / this.displayTileSize),
        y: Math.floor(e.offsetY / this.displayTileSize),
        rotation: this.rotation
      }
      this.$emit('update:modelValue', selectedTile)
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
.Tileset-container {
  position: absolute;
  width: v-bind(tileSetWidth);
  height: v-bind(tileSetHeight);
  z-index: 0;
}
#tilesetImg{
  position: absolute;
  width: v-bind(tileSetWidth);
  z-index: 1;
}
</style>