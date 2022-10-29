<template>
  <div>
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item"><a class="page-link" href="#">Previous</a></li>
        <li
          class="page-item"
          v-for="page in totalPages"
          :key="page"
          @click="onPageChange(page)"
          :class="currentPage === page ? 'current-page' : ''"
        >
          <a class="page-link" href="#">{{ page }}</a>
        </li>
        <!-- <li class="page-item"><a class="page-link" href="#">2</a></li>
        <li class="page-item"><a class="page-link" href="#">3</a></li> -->
        <li class="page-item"><a class="page-link" href="#">Next</a></li>
      </ul>
    </nav>
  </div>
</template>
<script>
export default {
  name: "UsersListPagination",
  props: {
    totalPages: {
      type: Number,
      default: 0,
    },
    currentPage: {
      type: Number,
      default: 1,
    },
  },
};
</script>
<style scoped>
</style>