<template>
  <div class="navbar">
    <div @click="$router.push('/')">Vue 3</div>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')">
        Posts
<!--        <router-link to="">Posts</router-link>-->
      </my-button>
      <my-button @click="$router.push('/about')">
        About
<!--        <router-link to="/about">About</router-link>-->
      </my-button>
      <my-button @click="$router.push('/store')">
        Store
      </my-button>
    </div>
  </div>
</template>

<script>
import MyButton from "@/components/UI/MyButton";
export default {
  name: "Navbar",
  components: {MyButton}
}
</script>

<style scoped>
.navbar{
  height: 50px;
  background: #d7d7d7;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 15px;
}
.navbar__btns{
  display: flex;
  column-gap: 25px;
}
</style>