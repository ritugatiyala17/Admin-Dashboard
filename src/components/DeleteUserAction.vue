<template>
  <div @click="onDeleteUser">
    <i
      class="delete-user-single-button fa fa-trash"
      aria-hidden="true"
      @click="onDeleteUser"
    ></i>
  </div>
</template>

<script>
export default {
  name: "DeleteUserAction",
  methods: {
    onDeleteUser() {
      this.$emit("delete-user");
    },
  },
};
</script>
<style scoped>
.delete-user-single-button:hover {
  font-size: 1.3rem;
  color: red;
  transition: 0.2s ease-out;
}
</style>