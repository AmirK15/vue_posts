<template>
<h1>Post Page c ID: {{$route.params.id}}</h1>
  <div v-if="isLoading">Loading...</div>
  <div v-else>
    <h3>{{post.title}}</h3>
    <p>{{post.body}}</p>
  </div>
</template>
<script>
import axios from "axios";

export default {

  name: "PostIdPage",

  data() {
    return {
      post: {},
      isLoading: true
    }
  },

  methods: {
    async getPost(id) {
      try {
        const res = await axios(`https://jsonplaceholder.typicode.com/posts/${id}`)
        this.post = res.data
        console.log(this.post)
      } catch (err) {
        console.log(err.message)
      } finally {
        this.isLoading = false
      }
    }
  },

  mounted() {
    const id = this.$route.params.id;
    this.getPost(id)
  }

}
</script>

<style scoped>

</style>