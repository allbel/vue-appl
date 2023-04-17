<template>
  <span>
    <img src="@/assets/arrow-dropdown-top.png" alt="top" v-if="flag">
    <img src="@/assets/arrow-dropdown-bottom.png" alt="bottom" v-else>
  </span>
</template>

<script>
export default {
  name: 'ArrowDropdownToggle',
  props: ['flag']
}
</script>