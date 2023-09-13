<script>
import Navbar from "@/components/UI/Navbar.vue";

export default {
  components: {Navbar}
}
</script>

<template>
  <Navbar></Navbar>
  <div class="app">
    <router-view></router-view>
  </div>
</template>

<styles>
* {
margin: 0;
padding: 0;
box-sizing: border-box;
}

.app {
padding: 20px;
}
</styles>