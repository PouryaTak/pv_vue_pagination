<template>
  <div v-if="paginationData.totalPages / paginationData.perPage > 1" class="pv_pagination">
    <div class="pv_page_input" :class="pageInputToggler ? 'toggle_page_input':''">
      <form @submit.prevent="jumptoPage">
        <input ref="pv_input" type="number" v-model.number="page" />
        <button type="submit" class="pv_btn">Go</button>
      </form>
    </div>
    <button class="pv_action_btn" :class="paginationData.currentPage == 1 ? 'disabled' : ''" @click="gotoPage(paginationData.currentPage - 1)">
      <svg class="chev_right" width="10" height="6" viewBox="0 0 10 6" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M1 1L5 5L9 1" stroke="#021C24" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>
    <ul v-if="isLessthanSevenPages" class="flex_horizontal_align">
      <li v-for="i in paginationData.totalPages" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
    </ul>

    <ul v-else-if="isBoarderPagesSelected" class="flex_horizontal_align">
      <li v-for="i in 3" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
      <li class="pv_btn" @click="pageInputToggle">...</li>
      <li v-for="i in threeLastPages" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
    </ul>

    <ul v-else-if="isLastItemOfFirstBoardered" class="flex_horizontal_align">
      <li v-for="i in 3" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
      <li class="pv_btn" @click="gotoPage(4)">4</li>
      <li class="pv_btn" @click="pageInputToggle">...</li>
      <li v-for="i in twoLastPages" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
    </ul>

    <ul v-else-if="isFirstItemOfLastBoardered" class="flex_horizontal_align">
      <li v-for="i in 2" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
      <li class="pv_btn" @click="pageInputToggle">...</li>
      <li class="pv_btn" @click="gotoPage(paginationData.totalPages - 3)">
        {{ paginationData.totalPages - 3 }}
      </li>
      <li v-for="i in threeLastPages" :key="i" class="pv_btn" :class="paginationData.currentPage == i ? 'active' : ''" @click="gotoPage(i)">
        {{ i }}
      </li>
    </ul>

    <!-- <ul v-else-if="paginationData.currentPage == 1 && paginationData.totalPages < 0" class="flex_horizontal_align">
      <li class="pv_btn active">
        {{ paginationData.currentPage }}
      </li>
      <li class="pv_btn" @click="gotoPage(paginationData.currentPage + 1)">
        {{ paginationData.currentPage + 1 }}
      </li>
    </ul>
    <ul v-else-if="paginationData.currentPage !== 1 && paginationData.totalPages < 0" class="flex_horizontal_align">
      <li class="pv_btn" @click="gotoPage(paginationData.currentPage - 1)">
        {{ paginationData.currentPage - 1 }}
      </li>
      <li class="pv_btn active">
        {{ paginationData.currentPage }}
      </li>
      <li class="pv_btn" @click="gotoPage(paginationData.currentPage + 1)">
        {{ paginationData.currentPage + 1 }}
      </li>
    </ul> -->

    <ul v-else class="flex_horizontal_align">
      <li class="pv_btn" @click="gotoPage(1)">
        {{ 1 }}
      </li>
      <li class="pv_btn" @click="pageInputToggle">...</li>
      <li class="pv_btn" @click="gotoPage(paginationData.currentPage - 1)">
        {{ paginationData.currentPage - 1 }}
      </li>
      <li class="pv_btn active">
        {{ paginationData.currentPage }}
      </li>
      <li class="pv_btn" @click="gotoPage(paginationData.currentPage + 1)">
        {{ paginationData.currentPage + 1 }}
      </li>
      <li class="pv_btn" @click="pageInputToggle">...</li>
      <li class="pv_btn" @click="gotoPage(paginationData.totalPages)">
        {{ paginationData.totalPages }}
      </li>
    </ul>
    <button class="pv_action_btn" :class="paginationData.currentPage == paginationData.totalPages ? 'disabled' : ''" @click="gotoPage(paginationData.currentPage + 1)">
      <svg class="chev_left" width="10" height="6" viewBox="0 0 10 6" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M1 1L5 5L9 1" stroke="#021C24" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>
  </div>
</template>
<script>
export default {
  name: "Pagination",
  props: {
    paginationData: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      page: "",
      pageInputToggler: false,
    };
  },
  computed: {
    threeLastPages() {
      return [this.paginationData.totalPages - 2, this.paginationData.totalPages - 1, this.paginationData.totalPages];
    },
    twoLastPages() {
      return [this.paginationData.totalPages - 1, this.paginationData.totalPages];
    },
    // conditions
    isLessthanSevenPages() {
      return this.paginationData.totalPages <= 7 && this.paginationData.totalPages > 0;
    },
    isBoarderPagesSelected() {
      return this.paginationData.totalPages > 7 && (this.paginationData.currentPage <= 3 || (this.paginationData.currentPage > this.paginationData.totalPages - 3 && this.paginationData.totalPages > 0)) && !this.isLastItemOfFirstBoardered && !this.isFirstItemOfLastBoardered;
    },
    isLastItemOfFirstBoardered() {
      return this.paginationData.currentPage === 3;
    },
    isFirstItemOfLastBoardered() {
      return this.paginationData.currentPage === this.paginationData.totalPages - 2;
    },
  },
  methods: {
    gotoPage(page) {
      if (page === this.paginationData.currentPage) {
        return;
      }
      this.$emit("gotoPage", page);
    },
    pageInputToggle() {
      this.pageInputToggler = !this.pageInputToggler;
      this.$refs.pv_input.focus();
    },
    jumptoPage() {
      if (!this.page || this.page <= 0 || this.page > this.paginationData.totalPages) {
        return;
      }
      this.gotoPage(this.page);
      this.pageInputToggler = false;
      this.page = "";
    },
  },
};
</script>
<style scoped>
* {
  box-sizing: border-box;
    transition: all .3s ease;
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
/* ------------------------------------ */
.pv_pagination {
  /* css variables used here because of scoped style limitation */
  --text: #252525;
  --active_color: #4b6cff;
  --white: #fff;
  --disable: #ececec;
  --border: #cce4eb;
  --hover: #f5fdff;
  /* ---------------------------------------------------------- */
  display: flex;
  align-items: center;
  width: max-content;
  margin: 0 auto;
  color: var(--text);
  position: relative;
}
.pv_btn,
.pv_action_btn {
  width: 2.5rem;
  height: 2.5rem;
  background: var(--white);
  border: 1px solid var(--border);
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 0.5rem;
  margin: 0 0.5rem;
  cursor: pointer;
  /* margin-left: 1.75rem; */
}

.pv_page_input {
  width: max-content;
  position: absolute;
  border: var(--disable) 1px solid;
  background: var(--white);
  border-radius: 1rem;
  padding: 0.5rem;
  opacity: 0;
  bottom: 0;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.07);
  left: 50%;
  transform: translateX(-50%);
  z-index: -1;
}
.pv_page_input form {
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

.pv_page_input form button {
  height: 2rem;
  color: var(--white);
  background: var(--text);
}
.pv_page_input form button:hover {
  background: var(--active_color);
}

.pv_page_input form input {
  width: 3rem;
  height: 2rem;
  font-size: 1rem;
  text-align: center;
  color: var(--text);
  border: 1px solid var(--border);
  border-radius: 0.5rem;
}
.pv_page_input form input:focus {
  outline: none;
  border: 1px solid var(--active_color);
}

.toggle_page_input {
  bottom: -2.5rem;
  opacity: 1;
}

.pv_btn {
  margin: 0;
}

.pv_btn:hover,
.pv_action_btn:hover {
  background: var(--hover);
}

.chev_left {
  transform: rotate(-90deg);
}
.chev_right {
  transform: rotate(90deg);
}

.flex_horizontal_align {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0;
}

.disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: default;
  background: var(--disable);
}

/* li.page {
  width: 2.5rem;
  height: 2.5rem;
  border-radius: 0.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 0.25rem;
  cursor: pointer;
} */

.pv_btn.active {
  background: var(--active_color);
  color: var(--white);
  border: none;
}
</style>
