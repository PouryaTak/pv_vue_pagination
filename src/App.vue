<template>
  <Pagination :paginationData="pagination" @gotoPage="goToPage" />
</template>

<script>
import Pagination from "./components/Pagination.vue";

export default {
  name: "App",
  components: {
    Pagination,
  },
  data() {
    return {
      pagination: {
        totalPages: 12,
        currentPage: 1,
        perPage: 0,
      },
    };
  },
  methods: {
    goToPage(page) {
      this.pagination.currentPage = page;
    },
  },
};
</script>
