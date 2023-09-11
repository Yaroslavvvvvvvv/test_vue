<script>
import PostItem from "@/components/PostItem.vue";

export default {
  components: {PostItem},
  props: {
    posts: {
      type: Array,
      required: true
    }
  }
}

</script>

<template>
  <div v-show="posts.length > 0">
    <h3>Список користувачів</h3>
    <post-item v-for="post in posts"
               :post="post"
               :key="post.id"
               @remove="$emit('remove', post)"
    />
  </div>
  <h2 v-show="posts.length === 0" style="color: red">Список постів пустий</h2>
</template>

<style scoped>

</style>