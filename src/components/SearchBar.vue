<template>
  <div class="search-container">
    <div class="input-container">
      <input
        type="text"
        v-model="searchString"
        placeholder="Search by name, email or role"
      />
      <i
        v-if="searchString.length"
        class="fa fa-x fa-xs search-icon cursor-pointer"
        @click="onClearSearch"
      ></i>
      <i v-else class="fa fa-search search-icon"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: {
    searchedKeyword: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      searchString: this.searchedKeyword,
    };
  },
  methods: {
    onClearSearch() {
      this.searchString = "";
    },
  },
  watch: {
    searchString(newVal) {
      this.$emit("search-user", newVal);
    },
    searchedKeyword() {
      this.searchString = this.searchedKeyword;
    },
  },
};
</script>

<style scoped>
.search-container {
  /* width: 100%; */
  display: flex;
  flex-direction: row;
  justify-content: end;
  align-items: center;
  margin: 0;
  padding: 0;
}
.input-container {
  border-radius: 4px;
  border: 1px lightgray solid;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}
input {
  min-width: 200px;
  border: none;
  border-radius: 4px;
  padding: 0.4rem 0.8rem;
  outline: none;
}
input::placeholder {
  font: 13px sans-serif;
  text-overflow: ellipsis;
}
.search-icon {
  padding-right: 1rem;
}
.fa-search {
  -webkit-text-stroke: 1px white;
}
</style>