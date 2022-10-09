<template>
  <div>
    <span class="users-count">Showing {{ users }} users</span>
  </div>
</template>

<script>
export default {
  name: "UserStatus",
  props: {
    users: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style scoped>
div {
  width: 160px;
  font-size: 14px;
  margin-bottom: 0.6rem;
}
.users-count {
  background-color: white;
  color: #015cc8;
  border-radius: 4px;
  border: 1px solid #015cc8;
  padding: 0.4rem 0.8rem;
}
</style>