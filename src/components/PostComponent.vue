<template>
  <div class="container">
    <h1>Posts</h1>
    <hr>

    <p v-if="!isLoaded" style="margin-top: 3em;">Loading...</p>

    <div v-else class="posts">
      <label for="create-post">Create Post</label><br>
      <input type="text" id="create-post" v-model="text"><button v-on:click="createPost">Post</button>
      <p class="error" v-if="error">{{ error }}</p>
      <div class="posts-container">
        <div class="post" 
        v-for="(post, index) in posts"
        v-bind:item="post"
        v-bind:index="index"
        v-bind:key="post._id">
          <p>{{ post.text }}</p>
          <sub>{{ post.createdAt }}</sub><br>
          <a href="#" v-on:click="deletePost(post._id)">Delete</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      text: '',
      isLoaded: false,
      url: "http://localhost:5000/api/posts"
    }
  },
  async mounted() {
    try {
      this.loadPosts();
    } catch(err) {
        this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      this.isLoaded = false;
      const textSend = this.text;
      await axios.post(this.url, {
          "text": textSend
      })
      this.posts = [];
      this.loadPosts();
    },
    async deletePost(id){
      this.isLoaded = false;
      await axios.delete(this.url+"/"+id);
      this.posts = [];
      this.loadPosts();
    },
    async loadPosts(){
      await axios.get(this.url).then(response => {
        this.posts = response.data;
        this.isLoaded = true;
      });
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
