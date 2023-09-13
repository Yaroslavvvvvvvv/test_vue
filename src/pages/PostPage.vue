<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";

export default {
  components: {MyInput, MySelect, MyButton, MyDialog, PostList, PostForm},
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
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
    changePage(pageNumber) {
      this.page = pageNumber
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers ['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert('Помилка')
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;

        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers ['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('Помилка')
      }
    },
  },
  mounted() {
    this.fetchPosts()
    console.log(this.$refs.observer)
    let options = {
      rootMargin: "0px",
      threshold: 1.0,
    };
    const callback = (entries, observer) => {
      if (entries[0]. isIntersecting && this.page < this.totalPages) {
        this.loadMorePosts()
      }
    }
    let observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer)
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {}
}
</script>
<template>
  <div>
    <h1>Сторінка з постами</h1>
    <MyInput
        v-model="searchQuery"
        placeholder="Пошук..."
    />
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
    <PostList v-bind:posts="sortedAndSearchedPosts"
              @remove="removePost"
              v-if="!isPostsLoading"
    />
    <div v-else>Завантаження...</div>
    <div ref="observer" class="observer"></div>
  </div>
</template>

<style>


.app__btns {
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
}

.observer {
  height: 30px;
  background: green;
}


</style>