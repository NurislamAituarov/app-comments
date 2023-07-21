<template>
  <ul class="page">
    <li
      class="page__btn"
      :class="{ active: currentPage != 1 }"
      @click="$emit('decrement-current-page')"
    >
      <svg
        width="20px"
        height="20px"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M15.7071 4.29289C16.0976 4.68342 16.0976 5.31658 15.7071 5.70711L9.41421 12L15.7071 18.2929C16.0976 18.6834 16.0976 19.3166 15.7071 19.7071C15.3166 20.0976 14.6834 20.0976 14.2929 19.7071L7.29289 12.7071C7.10536 12.5196 7 12.2652 7 12C7 11.7348 7.10536 11.4804 7.29289 11.2929L14.2929 4.29289C14.6834 3.90237 15.3166 3.90237 15.7071 4.29289Z"
          fill="#000000"
        />
      </svg>
    </li>

    <li
      class="page__numbers"
      @click="$emit('set-page', 1)"
      v-if="currentPage > 3"
    >
      {{ pages[0] }}
    </li>
    <li class="page__dots" v-if="currentPage > 3">...</li>

    <li
      class="page__numbers"
      v-for="pageNumber in displayedPages"
      :key="pageNumber"
      @click="$emit('set-page', pageNumber)"
      :class="{ active: currentPage === pageNumber }"
    >
      {{ pageNumber }}
    </li>

    <li class="page__dots" v-if="currentPage < pages.length - 2">...</li>

    <li
      class="page__numbers"
      @click="$emit('set-page', pages.length)"
      v-if="currentPage < pages.length - 2"
    >
      {{ pages[pages.length - 1] }}
    </li>

    <li
      class="page__btn page__btn-right"
      :class="{ active: currentPage < pages.length }"
      @click="$emit('increment-current-page')"
    >
      <svg
        width="20px"
        height="20px"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M15.7071 4.29289C16.0976 4.68342 16.0976 5.31658 15.7071 5.70711L9.41421 12L15.7071 18.2929C16.0976 18.6834 16.0976 19.3166 15.7071 19.7071C15.3166 20.0976 14.6834 20.0976 14.2929 19.7071L7.29289 12.7071C7.10536 12.5196 7 12.2652 7 12C7 11.7348 7.10536 11.4804 7.29289 11.2929L14.2929 4.29289C14.6834 3.90237 15.3166 3.90237 15.7071 4.29289Z"
          fill="#000000"
        />
      </svg>
    </li>
  </ul>
</template>


<script>
export default {
  props: {
    totalPage: { type: Number, default: 0 },
    currentPage: { type: Number, default: 1 },
  },

  data() {
    return {
      perPage: 10,
      pages: [],
    };
  },

  methods: {
    pagination(items) {
      let currentPage = this.currentPage;
      let perPage = this.perPage;

      let start = currentPage * perPage - perPage;
      let end = currentPage * perPage;

      return items.slice(start, end);
    },

    // generate number of pages
    setPages() {
      let numberOfPages = Math.ceil(this.items.length / this.perPage);

      for (i = 1; i <= numberOfPages; i++) {
        this.pages.push(i);
      }
    },
  },

  computed: {
    displayedItems() {
      return this.pagination(this.items);
    },
    displayedPages() {
      const res = [];

      for (let i = 0; i < this.totalPage; i++) {
        res.push(i + 1);
      }

      this.pages = res;
      if (this.currentPage === 1) {
        return this.pages.slice(this.currentPage - 1, this.currentPage + 4);
      } else if (this.currentPage === this.pages.length) {
        return this.pages.slice(this.currentPage - 5, this.currentPage + 1);
      } else if (this.currentPage >= 4 && this.currentPage <= 7) {
        return this.pages.slice(this.currentPage - 2, this.currentPage + 1);
      } else if (this.currentPage > 7) {
        return this.pages.slice(this.currentPage - 4, this.currentPage + 1);
      } else {
        return this.pages.slice(this.currentPage - 2, this.currentPage + 3);
      }
    },
  },

  watch: {
    items() {
      this.setPages();
    },
  },
};
</script>


<style scoped>
ul {
  list-style-type: none;
}

.page {
  width: 60%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 5rem;
  margin: 1rem;
  border-radius: 0.6rem;
  background: #ffffff;
  box-shadow: 0 0.8rem 2rem rgba(#5a6181, 0.05);
}

.page__numbers,
.page__btn,
.page__dots {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0.4rem;
  font-size: 1.2rem;
  cursor: pointer;
}
.page__dots {
  width: 2.6rem;
  height: 2.6rem;
  color: var(--greyLight);
  cursor: initial;
}
.page__numbers {
  width: 2.6rem;
  height: 2.6rem;
  border-radius: 0.4rem;
}
.page__numbers:hover {
  color: var(--primary);
}
.page__numbers.active {
  color: #ffffff;
  background: var(--primary);
  font-weight: 600;
  border: 1px solid var(--primary);
}

.page__btn {
  color: var(--greyLight);
  pointer-events: none;
  opacity: 0.2;
}
.page__btn.active {
  color: var(--greyDark);
  pointer-events: initial;
  opacity: 1;
}
.page__btn:hover {
  color: var(--primary);
}

.page__btn-right {
  transform: rotateY(180deg);
}

@media screen and (max-width: 420px) {
  .page {
    width: 100%;
  }
}
</style>