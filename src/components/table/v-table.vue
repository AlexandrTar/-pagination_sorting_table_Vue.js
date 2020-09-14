<template>
  <div class="v-table">
    <div class="v-table__header">
      <p @click="sortByName" title="click here to sort alphabetically">Name</p>
      <p @click="sortByPointsEarned" title="click here to sort points earned">Points earned</p>
      <p @click="sortByPointsSpent" title="click here to sort points spent">Points spent</p>
      <p @click="sortByPointsDate" title="click here to sort date">Registration date</p>
    </div>
    <div class="v-table__body">
      <v-table-row
        v-for="row in paginatedUsers"
        :key="row.id"
        :row_data="row"
      />
    </div>
    <div class="v-table__pagination">
      <div
          class="page"
          v-for="page in pages"
          :key="page"
          @click="pageClick(page)"
          :class="{'page__selected': page === pageNumber}"
      >
        {{page}}
      </div>
    </div>
  </div>
</template>

<script>
import vTableRow from './v-table-row'

export default {
name: "v-table",
  components: {
    vTableRow
  },
  props: {
    users_data: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data() {
    return{
      usersPerPage: 10,
      pageNumber: 1
    }
  },
  computed: {
    pages() {
      return Math.ceil(this.users_data.length / 10);
    },
    paginatedUsers() {
      let from = (this.pageNumber - 1) * this.usersPerPage
      let to = from + this.usersPerPage
      return this.users_data.slice(from, to)
    }
  },
  methods: {
    pageClick(page) {
      this.pageNumber = page
    },
    sortByName() {
      this.users_data.sort((a, b) => a.name.localeCompare(b.name))
    },
    sortByPointsEarned() {
      this.users_data.sort((a, b) => a.points_earned - b.points_earned)
    },
    sortByPointsSpent() {
      this.users_data.sort((a, b) => a.points_spent - b.points_spent)
    },
    sortByPointsDate() {
      this.users_data.sort((a, b) => a.registration_date.localeCompare(b.registration_date))
    }

  }

}
</script>

<style scoped>
.v-table {
  max-width: 900px;
  margin: 0 auto;
}
  .v-table__header{
    display: flex;
    justify-content: space-around;
    border-bottom: solid 1px grey;
  }
  .v-table__header p {
    flex-basis: 25%;
    text-align: left;
    white-space: nowrap;
    cursor: pointer;
  }
  .v-table__pagination {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 30px;
  }
  .page {
    padding: 8px;
    border: solid 1px grey;
    cursor: pointer;
    margin: 0 5px;
  }
  .page:hover {
    background: lightgrey;
  }
  .page__selected {
    background: lightgrey;
  }
</style>