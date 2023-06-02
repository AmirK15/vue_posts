<template>
  <form @submit.prevent>
    <h3>Create Post</h3>
    <!--        v-bind:value="post.title" @input="post.title = $event.target.value"-->
    <my-input
        v-focus
        v-model="post.title"
        placeholder="Name"
    />
    <my-input
        v-model="post.body"
        placeholder="Option"
    />
    <my-button
        style="align-self: flex-end; margin-top: 15px"
        @click="createPost"
    >
      Create
    </my-button>
  </form>
</template>

<script>

export default {
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

<style scoped>
form{
  display: flex;
  flex-direction: column;
}
</style>