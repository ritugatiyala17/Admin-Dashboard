<template>
  <div class="empty-state">
    <p v-if="!isSearchString">No users available.</p>
    <div v-else>
      <p>No users found with a searched keyword</p>
      <p>Try again with a different search</p>
      <button @click="onClearSearch" class="clear-search">Clear Search</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EmptyState",
  props: {
    isSearchString: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onClearSearch() {
      this.$emit("search-user", "");
    },
  },
};
</script>
<style scoped>
.empty-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  height: 450px;
  width: 100vmax;
}
.clear-search {
  min-width: 170px;
  border: 1px solid red;
  border-radius: 4px;
  padding: 0.25rem 0.5rem;
  background-color: white;
  color: red;
  font-size: 14px;
}
</style>