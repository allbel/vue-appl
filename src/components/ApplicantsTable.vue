<template>
  <div>
    <div class="searchBox">
      <input
        type="text"
        v-model="searchText"
        @input="filterByName"
        class="searchField"
        placeholder="Поиск"
      />
    </div>
    <div class="tableBox">
      <div class="mobile">
        <div
          class="select"
          :class="{ active: isDropdownOpen }"
          @click="toggleDropdown"
        >
          <div class="current" v-if="selectedOptionTitle || isDropdownOpen">
            <div class="sort">Сортировать по</div>
            <div class="value">
              {{ selectedOptionTitle ? selectedOptionTitle : "Сортировать по" }}
            </div>
            <div class="list" v-if="isDropdownOpen">
              <div @click="selectOption('ФИО', 'name')">ФИО</div>
              <div @click="selectOption('Дата подачи заявления', 'date')">
                Дата подачи заявления
              </div>
              <div @click="selectOption('Балл по русскому', 'rus')">
                Балл по русскому
              </div>
              <div @click="selectOption('Балл по математике', 'math')">
                Балл по математике
              </div>
              <div @click="selectOption('Балл по информатике', 'inf')">
                Балл по информатике
              </div>
              <div @click="selectOption('Суммарный балл', 'sum')">
                Суммарный балл
              </div>
              <div @click="selectOption('Процент', 'percent')">Процент</div>
            </div>
          </div>
          <div class="default" v-else>Сортировать по</div>
          <ArrowDropdownToggle v-bind:flag="isDropdownOpen" />
        </div>
        <span class="arrowsMobileToggle">
          <span
            class="arrowMobile"
            @click="sortMobileColumn(selectedOptionColumn)"
            :class="{ desc: sortOrderMobile === 'asc' }"
          >
            <svg
              width="7"
              height="8"
              viewBox="0 0 7 8"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M3.5 8L7 4L4.76995e-08 4L3.5 8Z"
                class="colorFill"
                :class="{ colorFillActive: sortOrderMobile === 'asc' }"
              />
              <path
                d="M3.5 0L3.5 4"
                class="colorStroke"
                stroke-width="2"
                :class="{ colorStrokeActive: sortOrderMobile === 'asc' }"
              />
            </svg>
          </span>
          <span
            class="arrowMobile"
            @click="sortMobileColumn(selectedOptionColumn)"
            :class="{ desc: sortOrderMobile === 'desc' }"
          >
            <svg
              width="7"
              height="8"
              viewBox="0 0 7 8"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M3.5 -1.5299e-07L7 4L-1.74846e-07 4L3.5 -1.5299e-07Z"
                class="colorFill"
                :class="{ colorFillActive: sortOrderMobile === 'desc' }"
              />
              <path
                d="M3.5 8L3.5 4"
                class="colorStroke"
                stroke-width="2"
                :class="{ colorStrokeActive: sortOrderMobile === 'desc' }"
              />
            </svg>
          </span>
        </span>
      </div>
      <table class="table">
        <tr class="rowHead">
          <th>
            <span @click="sortColumn('name')">
              ФИО
              <ArrowToggle
                v-if="sortOrder.name !== 'none'"
                v-bind:flag="sortOrder.name === 'asc'"
              />
            </span>
          </th>
          <th>
            <span @click="sortColumn('date')">
              Дата подачи заявления
              <ArrowToggle
                v-if="sortOrder.date !== 'none'"
                v-bind:flag="sortOrder.date === 'asc'"
              />
            </span>
          </th>
          <th>
            <span @click="sortColumn('rus')">
              Балл по русскому
              <ArrowToggle
                v-if="sortOrder.rus !== 'none'"
                v-bind:flag="sortOrder.rus === 'asc'"
              />
            </span>
          </th>
          <th>
            <span @click="sortColumn('math')">
              Балл по математике
              <ArrowToggle
                v-if="sortOrder.math !== 'none'"
                v-bind:flag="sortOrder.math === 'asc'"
              />
            </span>
          </th>
          <th>
            <span @click="sortColumn('inf')">
              Балл по информатике
              <ArrowToggle
                v-if="sortOrder.inf !== 'none'"
                v-bind:flag="sortOrder.inf === 'asc'"
              />
            </span>
          </th>
          <th>
            <span @click="sortColumn('sum')">
              Суммарный балл
              <ArrowToggle
                v-if="sortOrder.sum !== 'none'"
                v-bind:flag="sortOrder.sum === 'asc'"
              />
            </span>
          </th>
          <th>
            <span @click="sortColumn('percent')">
              Процент
              <ArrowToggle
                v-if="sortOrder.percent !== 'none'"
                v-bind:flag="sortOrder.percent === 'asc'"
              />
            </span>
          </th>
        </tr>
        <ApplicantsRow
          v-for="applicant of filteredApplicants"
          v-bind:applicant="applicant"
          v-bind:key="applicant.id"
        />
      </table>
    </div>
  </div>
</template>

<script>
import ApplicantsRow from "@/components/ApplicantsRow.vue";
import ArrowToggle from "@/components/ArrowToggle.vue";
import ArrowDropdownToggle from "@/components/ArrowDropdownToggle.vue";

export default {
  name: "ApplicantsTable",
  props: {
    applicants: {
      type: Array,
      required: true,
    },
  },
  components: {
    ArrowDropdownToggle,
    ApplicantsRow,
    ArrowToggle,
  },
  data() {
    return {
      searchText: "",
      sortOrder: {
        name: "asc",
        date: "none",
        rus: "none",
        math: "none",
        inf: "none",
        sum: "none",
        percent: "none",
      },
      selectedOptionTitle: "",
      selectedOptionColumn: "",
      isDropdownOpen: false,
      sortOrderMobile: "",
    };
  },
  computed: {
    filteredApplicants() {
      const searchKeywords = this.searchText.toLowerCase().split(" ");

      return this.applicants
        .filter((applicant) => {
          applicant.sum = 0;
          applicant.subjects.forEach((item) => {
            if (item.subject === "Русский язык") {
              applicant.rus = Number(item.score);
            }
            if (item.subject === "Математика") {
              applicant.math = Number(item.score);
            }
            if (item.subject === "Информатика") {
              applicant.inf = Number(item.score);
            }
            applicant.sum += Number(item.score);
          });
          applicant.percent = Math.round((100 * applicant.sum) / 15);

          applicant.color = "white";
          if (applicant.percent >= 50 && applicant.percent < 75) {
            applicant.color = "orange";
          } else if (applicant.percent >= 75) {
            applicant.color = "green";
          } else {
            applicant.color = "red";
          }

          return searchKeywords.some((keyword) =>
            applicant.name.toLowerCase().includes(keyword)
          );
        })
        .sort((a, b) => {
          const column = Object.keys(this.sortOrder).find(
            (key) => this.sortOrder[key] !== "none"
          );
          const order = this.sortOrder[column];
          const compare =
            typeof a[column] === "string"
              ? a[column].localeCompare(b[column])
              : a[column] - b[column];
          return order === "asc" ? compare : -compare;
        });
    },
  },
  methods: {
    filterByName() {
      this.$forceUpdate();
    },
    sortColumn(column) {
      if (this.sortOrder[column] === "asc") {
        this.filteredApplicants.sort((a, b) => {
          if (typeof a[column] === "string") {
            return a[column].localeCompare(b[column]);
          }
          if (typeof a[column] === "number") {
            return a[column] - b[column];
          }
        });
        this.sortOrder[column] = "desc";
        this.sortOrderMobile = "desc";
      } else {
        this.filteredApplicants.sort((a, b) => {
          if (typeof a[column] === "string") {
            return b[column].localeCompare(a[column]);
          }
          if (typeof a[column] === "number") {
            return b[column] - a[column];
          }
        });
        this.sortOrder[column] = "asc";
        this.sortOrderMobile = "asc";
      }

      Object.keys(this.sortOrder).forEach((key) => {
        if (key !== column) {
          this.sortOrder[key] = "none";
        }
      });
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
    selectOption(title, column) {
      this.selectedOptionTitle = title;
      this.selectedOptionColumn = column;
      this.isDropdownOpen = true;
      this.sortOrderMobile = "";
    },
    sortMobileColumn(column) {
      if (this.selectedOptionColumn) {
        this.sortColumn(column);
      }
    },
  },
};
</script>

<style lang="scss">
@import "../styles/style";

.searchBox {
  padding: 16px 16px 16px 38px;
  background-color: $color-white;
  border: 1px solid $color-blue-light;
  border-radius: 4px;
  position: relative;
  margin-bottom: 14px;

  &:hover {
    border-color: $color-blue-middle;
  }

  &:active {
    border-color: $color-blue;
  }

  &::after {
    content: url("@/assets/search.png");
    width: 15px;
    height: 16px;
    position: absolute;
    top: 50%;
    left: 16px;
    transform: translate(0, -50%);
  }
}

.searchField {
  outline: none;
  border: none;
  width: 100%;
  @include bigText($color-black);

  &::-webkit-input-placeholder {
    color: $color-medium;
  }

  &::-moz-placeholder {
    color: $color-medium;
  }

  &:-ms-input-placeholder {
    color: $color-medium;
  }
}

.tableBox {
  overflow-x: auto;

  &::-webkit-scrollbar {
    width: 24px;
    height: 8px;
    background-color: $color-blue-super-light;
  }

  &::-webkit-scrollbar-thumb {
    background-color: $color-blue-light;
    border-radius: 9em;
    box-shadow: inset 1px 1px 10px $color-blue-light;
  }

  &::-webkit-scrollbar-thumb:hover {
    background-color: #253861;
  }
}

.table {
  text-align: center;
  border-collapse: collapse;

  @media screen and (max-width: 767px) {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
}

tr {
  @media screen and (max-width: 767px) {
    display: flex;
    flex-direction: column;
  }
}

.rowHead {
  @media screen and (max-width: 767px) {
    display: none;
  }
}

th {
  padding: 10px 20px;
  white-space: nowrap;
  @include captionBoldText($color-blue);

  &:first-child {
    text-align: left;
    width: 100%;
  }

  & span {
    position: relative;
    &:hover {
      color: $color-blue-dark;
      cursor: pointer;
    }
  }
}

.mobile {
  display: none;

  @media screen and (max-width: 767px) {
    display: flex;
    gap: 12px;
  }

  .select {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 12px;
    height: 44px;
    background-color: $color-white;
    border: 1px solid $color-blue-light;
    border-radius: 4px;
    cursor: pointer;
    position: relative;
    margin-bottom: 8px;
    flex-grow: 1;

    .current {
      .sort {
        @include captionMiniText($color-medium);
      }

      .value {
        @include normalText($color-black);
      }

      .list {
        position: absolute;
        width: 100%;
        left: 0;
        top: calc(100% + 4px);
        border: 1px solid $color-blue-light;
        border-radius: 4px;
        z-index: 1;
        @include normalText($color-black);

        div {
          padding: 10px 12px;
          background-color: $color-white;
          border-bottom: 1px solid $color-blue-light;

          &:last-child {
            border-bottom: none;
          }

          &:hover {
            background-color: $color-blue-super-light;
          }
        }
      }
    }

    .default {
      @include normalText($color-medium);
    }
  }

  .active {
    border-color: $color-blue;
  }
}

.arrowsMobileToggle {
  display: flex;
  gap: 4px;

  .arrowMobile {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 44px;
    height: 44px;
    border: 1px solid $color-blue;
    border-radius: 2px;
    cursor: pointer;

    .colorFill {
      fill: $color-blue;
    }

    .colorStroke {
      stroke: $color-blue;
    }

    .colorFillActive {
      fill: $color-white;
    }

    .colorStrokeActive {
      stroke: $color-white;
    }
  }

  .desc {
    background-color: $color-blue;
  }
}
</style>
