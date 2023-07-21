<template>
  <div v-if="rows.length" class="container">
    <h1>Table comments</h1>

    <table class="table">
      <thead>
        <tr>
          <th @click="filterRows" class="head__id">
            Id
            <svg
              :class="{ 'head__id-svg': !arrow }"
              width="20px"
              height="15px"
              viewBox="0 0 1024 1024"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill="white"
                d="M572.235 205.282v600.365a30.118 30.118 0 1 1-60.235 0V205.282L292.382 438.633a28.913 28.913 0 0 1-42.646 0 33.43 33.43 0 0 1 0-45.236l271.058-288.045a28.913 28.913 0 0 1 42.647 0L834.5 393.397a33.43 33.43 0 0 1 0 45.176 28.913 28.913 0 0 1-42.647 0l-219.618-233.23z"
              />
            </svg>
          </th>
          <th>Name</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(row, index) in filteredRows.slice(
            pageStart,
            pageStart + countOfPage
          )"
          :key="index"
          @click="openCommentPage(row.id)"
        >
          <td>{{ row.id }}</td>
          <td>{{ row.name }}</td>
          <td>{{ row.email }}</td>
        </tr>
      </tbody>
    </table>

    <ThePagination
      :totalPage="totalPage"
      :currentPage="currPage"
      @set-page="setPage"
      @decrement-current-page="decrementCurrentPage"
      @increment-current-page="incrementCurrentPage"
    />
  </div>
</template>

<script>
import ThePagination from "./ThePagination.vue";

export default {
  components: { ThePagination },

  data() {
    return {
      filter_name: "",
      rows: [],
      currPage: 1,
      countOfPage: 10,
      oncePage: 0,
      arrow: false,
    };
  },

  async fetch() {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    ).then((res) => res.json());
    this.rows = await res;
  },

  computed: {
    filteredRows: function () {
      var filter_name = this.filter_name.toLowerCase();
      return this.filter_name.trim() !== ""
        ? this.rows.filter(function (d) {
            return d.name.toLowerCase().indexOf(filter_name) > -1;
          })
        : this.rows;
    },
    pageStart: function () {
      return (this.currPage - 1) * this.countOfPage;
    },
    totalPage: function () {
      return Math.ceil(this.filteredRows.length / this.countOfPage);
    },
  },

  mounted() {},

  methods: {
    setPage(pageNumber) {
      this.currPage = pageNumber;
    },

    decrementCurrentPage() {
      this.currPage--;
    },

    incrementCurrentPage() {
      this.currPage++;
    },

    filterRows() {
      this.arrow = !this.arrow;
      this.rows.reverse();
    },

    openCommentPage(id) {
      this.$router.push({
        path: "comments",
        query: { id },
      });
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 1020px;
  min-height: 90vh;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}
.container h1 {
  margin-bottom: 20px;
}

table {
  width: 100%;
  box-shadow: 0px 0px 2px 2px var(--greyLight);
}

table th {
  text-transform: uppercase;
  text-align: left;
  background: #44475c;
  color: #fff;
  padding: 8px;
  min-width: 30px;
}

table td {
  text-align: left;
  padding: 8px;
  height: 50px;
  font-size: 1rem;
}
table td:last-child {
  border-right: none;
}

table td:first-child {
  text-align: center;
}

table tbody tr:nth-child(2n) td {
  background: #d4d8f9;
}

table tbody tr {
  cursor: pointer;
}

.pagination {
  display: flex;
  max-width: 420px;
}
.page-item {
  min-width: 30px;
  padding: 5px;
  list-style: none;
  border: 1px solid #7d82a8;
  color: #7d82a8;
  display: flex;
  align-items: center;
  justify-content: center;
}
.page-item:hover {
  background-color: #7d82a8;
  cursor: pointer;
  color: white;
}
.page-item a {
  text-decoration: none;
  color: inherit;
}
.container__page-item {
  overflow: hidden;
}
.container__page-item-movies {
  display: flex;
  width: 100%;
}

.disabled {
  pointer-events: none;
}
.active {
  background-color: #007bff;
  color: white;
}

.head__id {
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
}
.head__id svg {
  transition: all 0.3s;
}
.head__id-svg {
  transform: rotateX(180deg);
}
.head__id:hover {
  background: #63657a;
  cursor: pointer;
}
</style>
