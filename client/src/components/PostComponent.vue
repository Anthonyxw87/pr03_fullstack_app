<template>
  <div class="container">
    <h1>Posts</h1>
    <!-- Create Post Here-->
    <div class="create-post">
      <label for="create-post"></label>
      <input type="text" id="create-post" v-model="text" placeholder="Create a post...">
      <button id="postbtn" v-on:click="createPost">Post!</button>
    </div>
    <hr>
    <p class="error" v-if="error">{{ error }}</p>
    <div class="posts-container">
      <div class="post" v-for="(post, index) in posts" v-bind:item="post" v-bind:index="index" v-bind:key="post._id"
        v-on:dblclick="deletePost(post._id)">
        <button id="deletebtn" v-on:click="deletePost(post._id)">Delete Post!</button>
        <div id="date">{{ `${post.createdAt.getMonth() + 1}/${post.createdAt.getDate()}/${post.createdAt.getFullYear()}` }}</div>
        <p class="text" id="text">{{ post.text }}</p>
      </div>
    </div>
  </div>

</template>

<script>
import PostService from '../PostService';
export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      text: ''
    }
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch (err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
      this.text ='';
    },
    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    },
  }
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.container {
  max-width: 800px;
  margin: 0 auto;
}

p.error {
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
  padding: 10px;
  margin-bottom: 15px;
}

div.post {
  position: relative;
  border: 10px solid gold;
  background-color: #bcffb8;
  padding: 10px 10px 30px 10px;
  margin-bottom: 15px;
}


div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15 px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 13px;
}

p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}

#postbtn {
  background-color: #008CBA;
  border-radius: 10rem;
  font-size: 22px;
  font-weight: bold;
  color: yellow;
  margin-left: 1%;
  transition: all 0.2s;
}

#postbtn:hover {
  border-radius: 20rem;
  font-size: 24px;
}


#create-post {
  width: 50%;
  padding: 6px;
  font-weight: bold;
  font-size: 16px;
  color: white;
  background-color: #008CBA;
}

::placeholder {
  color: white;
  font-weight: bold;
  font-size: 16px;
}

#deletebtn {
  padding: 0.3em 1.2em;
  position: absolute;
  top: 5%;
  right: 2%;
  border: 0.16em solid rgba(255, 255, 255, 0);
  border-radius: 100em;
  box-sizing: border-box;
  text-decoration: none;
  font-family: 'Roboto', sans-serif;
  font-weight: 300;
  color: white;
  font-weight: bold;
  text-shadow: 0 0.04em 0.04em rgba(0, 0, 0, 0.35);
  text-align: center;
  background-color: red;
  transition: all 0.2s;
}

#deletebtn:hover {
  border-color: black;
  font-size: 16px;
}
#date {
  position: absolute;
  top: 0;
  left: 0;
  font-size: 16px;
  width: 11%;
  height: auto;
  font-family:'Times New Roman', Times, serif;
  background-color: rgb(13, 114, 13);
  color: #dbe5e8;
}
</style>
