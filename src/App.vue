<template>
  <div id="app">
    <HeaderTop />
    <ContentBox />
  </div>
</template>

<script>
import HeaderTop from "@/components/HeaderTop.vue";
import ContentBox from "@/components/ContentBox.vue";

export default {
  name: "App",
  components: {
    HeaderTop,
    ContentBox,
  },
};
</script>

<style lang="scss">
@import "styles/style";

#app {
  font-family: "Proxima Nova", Avenir, Helvetica, Arial, sans-serif;
  color: $color-black;
}
</style>
