<template>
  <tr>
    <td>{{applicant.name}}</td>
    <td>{{applicant.date.split('-').reverse().join('.')}}</td>
    <td :class="getSpanClass(applicant.color)">
      {{applicant.rus}}
    </td>
    <td :class="getSpanClass(applicant.color)">
      {{applicant.math}}
    </td>
    <td :class="getSpanClass(applicant.color)">
      {{applicant.inf}}
    </td>
    <td :class="getSpanClass(applicant.color)">
      {{applicant.sum}}
    </td>
    <td>{{applicant.percent + '%'}}</td>
  </tr>
</template>

<script>

export default {
  name: 'ApplicantsRow',
  props: {
    applicant: {
      type: Object,
      required: true,
    }
  },
  components: {
    // ApplicantsCell,
  },
  methods: {
    getSpanClass(color) {
      return {
        'green': color === 'green',
        'red': color === 'red',
        'orange': color === 'orange'
      };
    }
  },
}
</script>

<style lang="scss">
@import "../styles/style";

td {
  width: auto;
  border: 1px solid $color-blue-light;
  text-align: left;
  padding: 24px 20px;
  background-color: $color-white;
  @include normalText($color-black);
}

td:last-child {
  text-align: center;
}

.green {
  @include normalBoldText($color-green);
}

.red {
  @include normalBoldText($color-red);
}

.orange {
  @include normalBoldText($color-orange);
}

</style>