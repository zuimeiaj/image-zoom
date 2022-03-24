<template>
  <div id="app">
    <div>
      <div class="wrapper-img">
        <YImage :size="size" :src="bg" />
      </div>
    </div>
  </div>
</template>

<script>
import YImage from "./components/zoom.vue";
import BG from "./assets/bg.jpg";
export default {
  name: "App",
  components: {
    YImage,
  },
  data() {
    return {
      bg: BG,
      size: {
        width: 200,
        height: 200,
      },
    };
  },
};
</script>

<style lang='less'>
.wrapper-img {
  width: 400px !important;
  height: 400px !important;

  border: 1px solid #ececec;
  border-radius: 4px;
  img {
    object-fit: contain;
  }
}
* {
  padding: 0;
  margin: 0;
}
</style>
