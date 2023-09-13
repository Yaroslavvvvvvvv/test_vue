<script setup>


</script>

<template>
<div class="navbar">
  <div @click="$router.push('/')">Vue 3</div>
  <div class="navbar__btns">
    <my-button @click="$router.push('/posts')">Пости</my-button>
    <my-button @click="$router.push('/about')" style="margin-left: 20px">Про сайт</my-button>
  </div>
</div>
</template>

<style scoped>
.navbar{
  align-item: center;
  height: 50px;
  background-color: aqua;
  box-shadow: 2px 2px 4px black;
  display: flex;
  padding:  15px;
}
.navbar__btns{
  margin-left: auto;
}
</style>