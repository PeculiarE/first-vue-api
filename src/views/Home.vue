<template>
  <div class="home">
    <div>
      <button class="button" @click="addPost">Add Post</button>
      <PostForm v-if="showForm" @addNewPost="handleAddNewPost"
      @editPost="handleEditPost" :postToBeEdited="currentPost" />
    </div>
    <h3 v-if="loading">Loading...</h3>
    <div class="posts">
      <div class="single__post" v-for="post in allPosts" :key="post.id">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
        <router-link :to="`/article/${post.id}`">Read more</router-link>
        <button class="button-two" @click="editForm(post)">Edit Post</button>
        <button class="button-three" @click="deletePost(post.id)">Delete Post</button>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import PostForm from '@/components/PostForm.vue';

export default {
  name: 'Home',
  data() {
    return {
      allPosts: [],
      loading: true,
      showForm: false,
      currentPost: {},
    };
  },
  components: {
    PostForm,
  },
  mounted() {
    const apiURL = 'https://jsonplaceholder.typicode.com/posts';
    fetch(apiURL)
      .then((response) => response.json())
      .then((response) => {
        console.log(response);
        this.allPosts = response;
        // this.loading = false;
      })
      .catch((error) => alert(error))
      .finally(() => {
        this.loading = false;
      });
  },
  methods: {
    addPost() {
      if (this.showForm) {
        this.currentPost = {};
      } else {
        this.showForm = !this.showForm;
        this.currentPost = {};
      }
    },
    handleAddNewPost(value) {
      this.showForm = false;
      this.allPosts.push(value);
    },
    editForm(post) {
      this.currentPost = post;
      this.showForm = true;
    },
    handleEditPost() {
      this.showForm = false;
      this.currentPost = {};
    },
    deletePost(id) {
      const apiURL = 'https://jsonplaceholder.typicode.com/posts';
      fetch(`${apiURL}/${id}`)
        .then((response) => response.json())
        .then((response) => {
          console.log(response);
          this.allPosts = this.allPosts.filter((el) => el.id !== id);
        })
        .catch((error) => alert(error));
    },
  },
};
</script>

<style >
.posts {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
}
.single__post {
  border: 1px solid #333333;
  border-radius: 5px;
  padding: 10px 10px 30px 10px;
  position: relative;
}
.button {
  padding: 10px 16px;
  box-sizing: border-box;
  margin-bottom: 30px;
  border-radius: 4px;
  color: white;
  background: blueviolet;
}
.button-two {
  position: absolute;
  width: 70px;
  height: 30px;
  padding: 5px;
  box-sizing: border-box;
  border-radius: 4px;
  margin-bottom: 30px;
  right: 100px;
  color: black;
  background: greenyellow;
}
.button-three {
  position: absolute;
  width: 85px;
  height: 30px;
  padding: 5px;
  box-sizing: border-box;
  border-radius: 4px;
  margin-bottom: 30px;
  right: 10px;
  color: black;
  background: greenyellow;
}
</style>
