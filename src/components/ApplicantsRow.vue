<template>
  <tr>
    <td>
      <span>ФИО</span>
      <span>{{ applicant.name }}</span>
    </td>
    <td>
      <span>Дата подачи заявления</span>
      <span>{{ applicant.date.split("-").reverse().join(".") }}</span>
    </td>
    <td :class="getTdClass(applicant.color)">
      <span>Балл по русскому</span>
      <span>{{ applicant.rus }}</span>
    </td>
    <td :class="getTdClass(applicant.color)">
      <span>Балл по математике</span>
      <span>{{ applicant.math }}</span>
    </td>
    <td :class="getTdClass(applicant.color)">
      <span>Балл по информатике</span>
      <span>{{ applicant.inf }}</span>
    </td>
    <td :class="getTdClass(applicant.color)">
      <span>Суммарный балл</span>
      <span>{{ applicant.sum }}</span>
    </td>
    <td>
      <span>Процент</span>
      <span class="percentBox">
        <div class="circle">
          <svg>
            <circle cx="20" cy="20" r="18"></circle>
            <circle
              cx="20"
              cy="20"
              r="18"
              :style="{
                strokeDashoffset:
                  'calc(114 - (114 * ' + applicant.percent + ') / 100)',
              }"
              :class="getCircleClass(applicant.color)"
            ></circle>
          </svg>
          <div class="number">
            {{ applicant.percent + "%" }}
          </div>
        </div>
        <div class="percentMobile">
          <div class="value">{{ applicant.percent + "%" }}</div>
          <div class="progress">
            <div
              class="progressLinear"
              :style="{ width: applicant.percent + '%' }"
              :class="getProgressClass(applicant.color)"
            ></div>
          </div>
        </div>
      </span>
    </td>
  </tr>
</template>

<script>
export default {
  name: "ApplicantsRow",
  props: {
    applicant: {
      type: Object,
      required: true,
    },
  },
  components: {
    // ApplicantsCell,
  },
  methods: {
    getTdClass(color) {
      return {
        green: color === "green",
        red: color === "red",
        orange: color === "orange",
      };
    },
    getCircleClass(color) {
      return {
        greenCircle: color === "green",
        redCircle: color === "red",
        orangeCircle: color === "orange",
      };
    },
    getProgressClass(color) {
      return {
        greenProgress: color === "green",
        redProgress: color === "red",
        orangeProgress: color === "orange",
      };
    },
  },
};
</script>

<style lang="scss">
@import "../styles/style";

td {
  width: auto;
  border: 1px solid $color-blue-light;
  text-align: left;
  padding: 10px 20px;
  background-color: $color-white;
  border-bottom-width: 4px;
  border-bottom-color: $color-blue-super-light;
  @include normalText($color-black);

  @media screen and (max-width: 767px) {
    display: flex;
    align-items: center;
    border-bottom-width: 1px;
    border-color: $color-blue-super-light;
    padding: 10px 12px;
  }

  & span {
    &:first-child {
      display: none;
      flex-basis: 155px;

      @media screen and (max-width: 767px) {
        display: inline-block;
        @include captionBoldText($color-medium);
      }
    }
    &:last-child {
      flex-grow: 1;
      width: 50%;
    }
  }
}

td:last-child {
  text-align: center;

  @media screen and (max-width: 767px) {
    text-align: left;
  }
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

.greenProgress {
  background-color: $color-green;
}

.redProgress {
  background-color: $color-red;
}

.orangeProgress {
  background-color: $color-orange;
}

.circle {
  display: inline-block;
  width: 40px;
  height: 40px;
  position: relative;

  @media screen and (max-width: 767px) {
    display: none;
  }

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

.percentMobile {
  display: none;

  @media screen and (max-width: 767px) {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .progress {
    height: 4px;
    border-radius: 2px;
    background-color: $color-blue-light;
    flex-grow: 1;

    .progressLinear {
      height: 4px;
      border-radius: 2px;
      //background-color: $color-orange;
      //width: 50%;
    }
  }
}
</style>
