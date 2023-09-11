<script>
import MyButton from "@/components/UI/MyButton.vue";
import MyInput from "@/components/UI/MyInput.vue";

export default {
  components: {MyInput, MyButton},
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: ''
      }
    }
  }
}

</script>

<template>
  <form @submit.prevent>
    <h4>Створення поста</h4>
    <MyInput
        v-model="post.title"
        type="text"
        placeholder="Назва"/>
    <MyInput
        v-model="post.body"
        type="text"
        placeholder="Опис"/>
    <MyButton
        class="btn"
        style="align-self: flex-end; margin-top: 15px"
        @click="createPost"
    >Створення
    </MyButton>
  </form>
</template>

<style scoped>

form {
  display: flex;
  flex-direction: column;
}


</style>