<template>
  <div id="app">
  <box-canvas :draw_mode="metaData.draw_mode"
              :show_annotations="metaData.show_annotations"
              :mouse_position="metaData.mouse_position"
              :canvas_transform="metaData.canvas_transform"
              :box_list="metaData.box_list"/>
  </div>
</template>

<script>
import BoxCanvas from "./boxes_assignment.vue";
import MetaData from './metadata.json';
export default {
  components: {
    'box-canvas': BoxCanvas
  },
  data() {
    return {
      metaData: Object
    };
  },
  created() {
    this.metaData = MetaData;
  },
  methods: {}
}
</script>

<style lang="scss">
@import './app.scss'
</style>
