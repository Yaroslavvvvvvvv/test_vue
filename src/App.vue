<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";

export default {
  components: {MyButton, MyDialog, PostList, PostForm},
  data() {
    return {
      posts: [],
      dialogVisible: false,
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
        this.posts = response.data
      } catch (e) {
        alert('Помилка')
      }
    }
  }
}
</script>
<template>
  <div class="app">
    <h1>Сторінка з постами</h1>
    <MyButton
        @click="showDialog"
    >Створити пост
    </MyButton>
    <MyDialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </MyDialog>
    <PostForm @create="createPost"/>
    <PostList v-bind:posts="posts"
              @remove="removePost"
    />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}


</style>