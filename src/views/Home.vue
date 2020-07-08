<template>
  <div class="home">
    <DrawFrame />
  </div>
</template>

<script>
// @ is an alias to /src
import DrawFrame from "./DrawFrame.vue";

export default {
  name: "Home",
  components: {
    DrawFrame
  }
};
</script>
<style lang="scss">
.home {
}
</style>
