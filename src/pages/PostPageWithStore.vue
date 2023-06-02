<template>
  <div>
    <h2>{{$store.state.likes}}</h2>
    <h1>Posts Page</h1>
    <my-input
        v-model="searchQuery"
        placeholder="Search..."
    />
    <div class="app__btns">
      <my-button @click="showDialog">
        Create Post
      </my-button>
      <my-select
          v-model="selectedSort"
          :options="selectedOptions"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>
    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"
    />
    <div v-else>Loading...</div>
    <div v-intersection="loadMorePosts" class="observer"></div>
    <!--    <div class="page__wrapper">-->
    <!--      <div-->
    <!--          v-for="pageNum in totalPages"-->
    <!--          :key="pageNum"-->
    <!--          class="page"-->
    <!--          :class="{-->
    <!--            'current__page' : page === pageNum-->
    <!--          }"-->
    <!--          @click="changePage(pageNum)"-->
    <!--      >-->
    <!--        {{ pageNum }}-->
    <!--      </div>-->
    <!--    </div>-->
  </div>
</template>

<script>

import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from 'axios'
import MyInput from "@/components/UI/MyInput";

export default {

  components: {
    MyInput,
    PostForm,
    PostList
  },

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
      selectedOptions: [
        {value: 'title', name: 'For Name'},
        {value: 'body', name: 'For Subtitle'}
      ]
    }
  },

  methods: {
    createPost(post) {
      this.posts = [...this.posts, post]
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(item => item.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    // changePage(pageNum) {
    //   this.page = pageNum
    // },
    async fetchPosts() {
      try {
        this.isPostsLoading = true
        const response = await axios('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data
      } catch (err) {
        alert(err.message)
      } finally {
        this.isPostsLoading = false
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1
        const response = await axios('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data]
      } catch (err) {
        alert(err.message)
      }
    }

  },

  mounted() {
    this.fetchPosts()

    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPages) {
    //     this.loadMorePosts()
    //   }
    // };
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer)

  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // }
  }
}
</script>

<style>


.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  cursor: pointer;
  border: 2px solid black;
  padding: 7px 10px;
}

.current__page{
  border: 2px solid #42bd87;
}

.observer{
  height: 30px;
  background: gray;
}
</style>

<!--Single file component-->