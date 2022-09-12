<template>
  <button class="btn" > <!-- slot - аналог children -->
  <slot/>
  </button>
</template>

<script>
export default {
  name: "MyButton"
}
</script>

<style scoped>
.btn {
  padding: 10px 15px;
  background-color: transparent;
  color: teal;
  border: 1px solid teal;
}
</style>
