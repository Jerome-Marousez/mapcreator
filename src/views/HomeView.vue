<template>

  <div class="home">

    <MapToolBar v-if="currentTab === 'mapEditor'" />

    <MetaDataToolBar v-if="currentTab === 'metadataEditor'">
      <template v-slot:metadata>
        <MetadataOverlay />
      </template>
      <template v-slot:default>
        <Tileset :source="tileSet" />
      </template>
    </MetaDataToolBar>

    <div>
      <Map />

      <div style="margin-top: 20px; display: flex">
        <div>
          <div class="color" style="background-color: hsl(350, 50%, 40%)" />
          <div class="color" style="background-color: hsl(350, 50%, 50%)" />
          <div class="color" style="background-color: hsl(350, 50%, 60%)" />
        </div>

        <div>
          <div class="color" style="background-color: hsl(20, 40%, 40%)" />
          <div class="color" style="background-color: hsl(20, 40%, 50%)" />
          <div class="color" style="background-color: hsl(20, 40%, 60%)" />
        </div>

        <div>
          <div class="color" style="background-color: hsl(130, 50%, 35%)" />
          <div class="color" style="background-color: hsl(130, 50%, 45%)" />
          <div class="color" style="background-color: hsl(130, 50%, 55%)" />
        </div>

        <div>
          <div class="color" style="background-color: hsl(105, 40%, 40%)" />
          <div class="color" style="background-color: hsl(105, 40%, 50%)" />
          <div class="color" style="background-color: hsl(105, 40%, 60%)" />
        </div>

        <div>
          <div class="color" style="background-color: hsl(200, 50%, 40%)" />
          <div class="color" style="background-color: hsl(200, 50%, 50%)" />
          <div class="color" style="background-color: hsl(200, 50%, 60%)" />
        </div>

        <div>
          <div class="color" style="background-color: hsl(220, 40%, 40%)" />
          <div class="color" style="background-color: hsl(220, 40%, 50%)" />
          <div class="color" style="background-color: hsl(220, 40%, 60%)" />
        </div>

        <div>
          <div class="color" style="background-color: hsl(200, 0%, 40%)" />
          <div class="color" style="background-color: hsl(200, 0%, 65%)" />
          <div class="color" style="background-color: hsl(200, 0%, 85%)" />
        </div>

        <div class="color" style="background-color: hsl(200, 0%, 25%)" />


      </div>
    </div>

  </div>

</template>

<script>
import Tileset from "@/components/Tileset"
import Map from "@/components/Map"
import MapToolBar from "@/components/MapToolBar"
import MetaDataToolBar from "@/components/MetaDataToolBar"
import MetadataOverlay from "@/components/MetadataOverlay"
import Room from "@/components/Room"

export default {
  name: 'HomeView',
  components: {
    MapToolBar,
    MetaDataToolBar,
    Tileset,
    MetadataOverlay,
    Map,
    Room,
  },

  inject: ['tileSize', 'tileSet'],

  data() {
    return {
      currentTab: 'mapEditor',
    }
  },

  computed: {
  },

  methods:{
  },
  mounted(){
  },
  created(){
  },
}
</script>

<style scoped>
.home {
  display: flex;
  justify-content: space-between;
  padding: 10px;
}

.color {
  width: 10px;
  height: 10px;
}
</style>
