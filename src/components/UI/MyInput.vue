<script>
export default {
  name: 'my-input',
  props: {
    modelValue: [String, Number]
  },
  methods: {
    updateInput (event) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<template>
  <input :value="modelValue"
         @input = "updateInput"
         type="text">
</template>

<style scoped>
input {
  width: 100%;
  border: 2px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
</style>