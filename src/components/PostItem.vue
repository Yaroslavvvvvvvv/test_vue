<script>
import MyButton from "@/components/UI/MyButton.vue";

export default {
  components: {MyButton},
  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>

<template>
  <div class="post">
    <div>
      <div>{{ post.id }}</div>
      <div><strong>Назва:</strong>{{ post.title }}</div>
      <div><strong>Опис:</strong>{{ post.body }}</div>
    </div>
    <div class="post__btns">
      <MyButton
          @click="$router.push(`/posts/${post.id}`)"
      >Відкрити</MyButton>
      <MyButton
          @click="$emit('remove', post)"
      >Видалити</MyButton>
    </div>
  </div>
</template>

<style scoped>
.post {
  padding: 15px;
  border: 1px solid;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.post__btns{
  display: flex;
}
</style>