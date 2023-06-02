<template>
  <input
      :value="modelValue"
      @input="updateValue"
      class="input"
      type="text"
  >
</template>

<script>
export default {
  name: "my-input",

  props: {
    modelValue: [String, Number]
  },

  methods: {
    updateValue(e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>

<style scoped>
.input{
  width: 100%;
  border: 1px solid #42bd87;
  padding: 10px 15px;
  margin-top: 15px;
}
</style>