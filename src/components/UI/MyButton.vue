<script>
export default {
  name: 'my-button'
}
</script>

<template>
<button>
<slot></slot>
</button>
</template>

<style scoped>
button {
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
}
</style>