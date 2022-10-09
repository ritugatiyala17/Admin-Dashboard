<template>
  <div class="loader-state">
    <i class="fa fa-spinner fa-xl fa-spin"></i>
  </div>
</template>

<script>
export default {
  name: "LoadingState",
};
</script>
<style scoped>
.loader-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  width: 100%;
  height: 100%;
}
</style>