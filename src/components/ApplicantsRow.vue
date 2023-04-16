<template>
  <tr>
    <td>{{applicant.name}}</td>
    <td>{{applicant.date.split('-').reverse().join('.')}}</td>
    <td :class="getTdClass(applicant.color)">
      {{applicant.rus}}
    </td>
    <td :class="getTdClass(applicant.color)">
      {{applicant.math}}
    </td>
    <td :class="getTdClass(applicant.color)">
      {{applicant.inf}}
    </td>
    <td :class="getTdClass(applicant.color)">
      {{applicant.sum}}
    </td>
    <td>
      <div class="circle">
        <svg>
          <circle cx="20" cy="20" r="18"></circle>
          <circle cx="20" cy="20" r="18"
                  :style="{ strokeDashoffset: 'calc(114 - (114 * ' + applicant.percent + ') / 100)' }"
                  :class="getCircleClass(applicant.color)"
          ></circle>
        </svg>
        <div class="number">
          {{applicant.percent + '%'}}
        </div>
      </div>
    </td>
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
    getTdClass(color) {
      return {
        'green': color === 'green',
        'red': color === 'red',
        'orange': color === 'orange'
      };
    },
    getCircleClass(color) {
      return {
        'greenCircle': color === 'green',
        'redCircle': color === 'red',
        'orangeCircle': color === 'orange'
      };
    },
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

.greenCircle {
  stroke: $color-green;
}

.redCircle {
  stroke: $color-red;
}

.orangeCircle {
  stroke: $color-orange;
}

.circle {
  display: inline-block;
  width: 40px;
  height: 40px;
  position: relative;

  & svg {
    box-sizing: border-box;
    width: 40px;
    height: 40px;
    position: relative;
    transform: rotate(-90deg);

    & circle {
      box-sizing: border-box;
      width: 40px;
      height: 40px;
      fill: none;
      transform: translate(0, 0);
      stroke-dasharray: 114;
      stroke-dashoffset: 114;
      stroke-linecap: round;

      &:nth-child(1) {
        stroke-dashoffset: 0;
        stroke-width: 1px;
        stroke: $color-blue-light;
        transform: translate(0, 0);
      }

      &:nth-child(2) {
        stroke-dashoffset: calc(114 - (114 * 50) / 100);
        stroke-width: 3px;
        transform: translate(0, 0);
      }
    }
  }

  .number {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    @include normalText($color-black);
  }
}

</style>