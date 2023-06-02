<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "my-button"
}
</script>

<style scoped>
.btn{
  padding: 10px 15px;
  background: transparent;
  color: #42bd87;
  border: 1px solid #42bd87;
  cursor: pointer;
}
</style>