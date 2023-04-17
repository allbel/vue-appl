<template>
  <span class="arrows">
    <img src="@/assets/arrow-top.png" alt="top" v-if="flag">
    <img src="@/assets/arrow-bottom.png" alt="bottom" v-else>
  </span>
</template>

<script>
export default {
  name: 'ArrowToggle',
  props: ['flag']
}
</script>

<style lang="scss">
.arrows {
  position: absolute;
  right: -9px;
}
</style>