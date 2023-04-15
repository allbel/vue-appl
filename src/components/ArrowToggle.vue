<template>
  <img src="@/assets/arrow-bottom.png" alt="bottom" v-if="flag">
  <img src="@/assets/arrow-top.png" alt="top" v-else>
</template>

<script>
export default {
  name: 'ArrowToggle',
  props: ['flag']
}
</script>