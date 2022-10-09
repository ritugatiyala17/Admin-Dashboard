<template>
  <div @click="onDeleteUser">
    <i class="fa fa-trash danger" aria-hidden="true"></i>
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