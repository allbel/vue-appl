<template>
  <div>
    <div class="searchBox">
      <input type="text" v-model="searchText" @input="filterByName" class="searchField" placeholder="Поиск">
    </div>
    <div class="tableBox">
      <table class="table">
        <tr>
          <th>
            <span @click="sortColumn('name')">
              ФИО <ArrowToggle v-if="sortOrder.name !== 'none'" v-bind:flag="sortOrder.name === 'asc'" />
            </span>
          </th>
          <th>
            <span @click="sortColumn('date')">
              Дата подачи заявления <ArrowToggle v-if="sortOrder.date !== 'none'" v-bind:flag="sortOrder.date === 'asc'" />
            </span>
          </th>
          <th>
            <span @click="sortColumn('rus')">
              Балл по русскому <ArrowToggle v-if="sortOrder.rus !== 'none'" v-bind:flag="sortOrder.rus === 'asc'" />
            </span>
          </th>
          <th>
            <span @click="sortColumn('math')">
              Балл по математике <ArrowToggle v-if="sortOrder.math !== 'none'" v-bind:flag="sortOrder.math === 'asc'" />
            </span>
          </th>
          <th>
            <span @click="sortColumn('inf')">
              Балл по информатике <ArrowToggle v-if="sortOrder.inf !== 'none'" v-bind:flag="sortOrder.inf === 'asc'" />
            </span>
          </th>
          <th>
            <span @click="sortColumn('sum')">
              Суммарный балл <ArrowToggle v-if="sortOrder.sum !== 'none'" v-bind:flag="sortOrder.sum === 'asc'" />
            </span>
          </th>
          <th>
            <span @click="sortColumn('percent')">
              Процент <ArrowToggle v-if="sortOrder.percent !== 'none'" v-bind:flag="sortOrder.percent === 'asc'" />
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
import ApplicantsRow from '@/components/ApplicantsRow.vue'
import ArrowToggle from '@/components/ArrowToggle.vue'

export default {
  name: 'ApplicantsTable',
  props: {
    applicants: {
      type: Array,
      required: true,
    }
  },
  components: {
    ApplicantsRow,
    ArrowToggle,
  },
  data() {
    return {
      searchText: '',
      sortOrder: {
        name: 'asc',
        date: 'none',
        rus: 'none',
        math: 'none',
        inf: 'none',
        sum: 'none',
        percent: 'none',
      }
    };
  },
  computed: {
    filteredApplicants() {
      const searchKeywords = this.searchText.toLowerCase().split(' ');

      return this.applicants.filter(applicant => {
        applicant.sum = 0;
        applicant.subjects.forEach(item => {
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
        applicant.percent = Math.round(100 * applicant.sum / 15);

        applicant.color = 'white';
        if (applicant.percent >= 50 && applicant.percent < 75) {
          applicant.color = 'orange';
        } else if (applicant.percent >= 75) {
          applicant.color = 'green';
        } else {
          applicant.color = 'red';
        }

        return searchKeywords.some(keyword => applicant.name.toLowerCase().includes(keyword));
      }).sort((a, b) => {
        const column = Object.keys(this.sortOrder).find(key => this.sortOrder[key] !== 'none');
        const order = this.sortOrder[column];
        const compare = typeof a[column] === 'string' ? a[column].localeCompare(b[column]) : a[column] - b[column];
        return order === 'asc' ? compare : -compare;
      });
    }
  },
  methods: {
    filterByName() {
      this.$forceUpdate();
    },
    sortColumn(column) {
      if (this.sortOrder[column] === 'asc') {
        this.filteredApplicants.sort((a, b) => {
          if (typeof a[column] === 'string') {
            return a[column].localeCompare(b[column]);
          }
          if (typeof a[column] === 'number') {
            return a[column] - b[column];
          }
        });
        this.sortOrder[column] = 'desc';
      } else {
        this.filteredApplicants.sort((a, b) => {
          if (typeof a[column] === 'string') {
            return b[column].localeCompare(a[column]);
          }
          if (typeof a[column] === 'number') {
            return b[column] - a[column];
          }
        });
        this.sortOrder[column] = 'asc';
      }

      Object.keys(this.sortOrder).forEach(key => {
        if (key !== column) {
          this.sortOrder[key] = 'none';
        }
      });
    }
  },
}
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
  white-space: nowrap;
}

th {
  padding: 10px 20px;
  @include captionBoldText($color-blue);

  &:first-child {
    text-align: left;
    width: 100%;
  }

  & span:hover {
    color: $color-blue-dark;
    cursor: pointer;
  }
}

</style>