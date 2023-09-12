<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";
import MySelect from "@/components/UI/MySelect.vue";

export default {
  components: {MySelect, MyButton, MyDialog, PostList, PostForm},
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      sortOptions: [
        {value: 'title', name: 'По назві'},
        {value: 'body', name: 'По змісту'}
      ]
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
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
        this.posts = response.data;
      } catch (e) {
        alert('Помилка')
      } finally {
        this.isPostsLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts()
  },
  computed: {
    sortedPosts(){
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    }
  },
  watch: {
  }
}
</script>
<template>
  <div class="app">
    <h1>Сторінка з постами</h1>
    <div class="app__btns">
      <MyButton
          @click="showDialog"
      >Створити пост
      </MyButton>
      <MySelect
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>
    <MyDialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </MyDialog>
    <PostList v-bind:posts="sortedPosts"
              @remove="removePost"
              v-if="!isPostsLoading"
    />
    <div v-else>Завантаження...</div>
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

.app__btns {
  display: flex;
  justify-content: space-between;
}


</style>