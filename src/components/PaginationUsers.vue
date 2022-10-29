<template>
  <div>
    <button
      @click="onPageChange(1)"
      class="first-page"
      :disabled="!activateFirstPage"
    >
      <i class="fa-solid fa-angles-left"></i>
    </button>
    <button
      @click="onPageChange(currentPage - 1)"
      :disabled="!activatePreviousPage"
    >
      <i class="fa-solid fa-angle-left"></i>
    </button>
    <span
      v-if="remaingingPages && currentPage !== 1"
      :disabled="true"
      class="show-more-pages"
    >
      ...
    </span>
    <button
      v-for="page in pagesToShow"
      :key="page"
      @click="onPageChange(page)"
      :class="currentPage === page ? 'current-page' : ''"
    >
      {{ page }}
    </button>
    <span
      v-if="remaingingPages && currentPage !== totalPages"
      :disabled="true"
      class="show-more-pages"
    >
      ...
    </span>
    <button
      @click="onPageChange(currentPage + 1)"
      :disabled="!activateNextPage"
    >
      <i class="fa-solid fa-angle-right"></i>
    </button>
    <button
      @click="onPageChange(totalPages)"
      class="last-page"
      :disabled="!activateLastPage"
    >
      <i class="fa-solid fa-angles-right"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "PagnationUsers",
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
  data() {
    return {
      maxUserPagesVisible: 3,
    };
  },
  computed: {
    activateFirstPage() {
      return this.currentPage !== 1;
    },
    activateLastPage() {
      return this.currentPage !== this.totalPages;
    },
    activatePreviousPage() {
      return this.currentPage !== 1;
    },
    activateNextPage() {
      return this.currentPage !== this.totalPages;
    },
    pagesToShow() {
      return Array(this.maxUserPagesVisible)
        .fill(0)
        .map((_, i) => i + 1);
    },
    remaingingPages() {
      return this.totalPages > this.pagesToShow.length;
    },
  },
  methods: {
    onPageChange(pageNum) {
      this.$emit("page-change", pageNum);
    },
  },
};
</script>

<style scoped>
button {
  border: 1px solid lightgray;
  border-radius: 50%;
  padding: 0.25rem 0.7rem;
  background-color: white;
  margin-right: 1rem;
  font-weight: 300;
}
.current-page {
  border: none;
  background-color: #015cc8;
  color: white;
}
.last-page,
.first-page {
  padding: 0.25rem 0.5rem;
}
.fa-solid {
  -webkit-text-stroke: 1px white;
}
.show-more-pages {
  padding-right: 1rem;
}
</style>