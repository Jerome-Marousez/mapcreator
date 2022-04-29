<template>

  <div class="MetaDataToolBar-container">
    <div>
      <slot name="metadata">

      </slot>
    </div>
    <div>
      <slot name="default">

      </slot>
    </div>
  </div>


</template>


<script>
export default {
  name: "MetaDataToolBar",

  inject: ['displayTileSize'],

  components: {},

  props: {},

  emits: [],

  data() {
    return {
    }
  },

  provide() {
    return {}
  },

  computed: {
  },

  methods: {
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
.MetaDataToolBar-container {
  position: relative;
}
</style>