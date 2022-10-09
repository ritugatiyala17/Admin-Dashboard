<template>
  <div class="actions-bar">
    <span class="mr-3 selected-info"
      ><span class="selected-users">{{ selectedUsers }} selected</span>
    </span>
    <button class="mr-3 delete-button" @click="onDeleteSelectedUsers">
      <i class="fa fa-trash fa-sm" aria-hidden="true"></i> Delete Selected
    </button>
    <span @click="onUnselectUsers" class="ml-1">
      <i class="fa-solid fa-x fa-xs cursor-pointer"></i>
    </span>
  </div>
</template>

<script>
export default {
  name: "ActionsBar",
  props: {
    selectedUsers: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    onDeleteSelectedUsers() {
      this.$emit("delete-selected-users");
    },
    onUnselectUsers() {
      this.$emit("unselect-users");
    },
  },
};
</script>

<style scoped>
.actions-bar {
  display: flex;
  flex-direction: row;
  justify-content: start;
  align-items: center;
  margin-bottom: 0.6rem;
}
svg {
  font-size: 48px;
}
mr-3 {
  margin-right: 3rem;
}
.delete-button {
  min-width: 170px;
  border: 1px solid red;
  border-radius: 4px;
  padding: 0.25rem 0.5rem;
  background-color: white;
  color: red;
  font-size: 14px;
}
.selected-info {
  width: 100px;
}
.selected-users {
  border-radius: 4px;
  background-color: #015cc8;
  color: white;
  padding: 0.4rem 0.8rem;
  font-size: 14px;
}
.ml-1 {
  margin-left: 1rem;
}
</style>