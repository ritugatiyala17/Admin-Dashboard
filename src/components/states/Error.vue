<template>
  <div class="error-state">
    <i class="fa-solid fa-triangle-exclamation"></i>
  </div>
</template>

<script>
export default {
  name: "Error",
};
</script>
<style scoped>
.error-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  width: 100%;
  height: 100%;
}
</style>