<template>
  <div>
    <form class="form" @submit.prevent="addPost">
      <div>
        <label for="title">Post Title: </label>
        <input type="text" name="title" id="title" v-model="newPost.title" />
      </div>
      <div>
        <label for="userId">Post User ID: </label>
        <input
          type="number"
          name="userId"
          id="userId"
          v-model="newPost.userId"
          :disabled="postToBeEdited.id"
        />
      </div>
      <div>
        <label for="body">Post Body:</label><br />
        <textarea
          name="body"
          id="body"
          cols="30"
          rows="10"
          v-model="newPost.body"
        ></textarea>
      </div>
      <button class="button">Send Post</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'PostForm',
  props: {
    postToBeEdited: {
      type: Object,
      required: false,
    },
  },
  data() {
    return {
      newPost: {
        userId: null,
        title: null,
        body: null,
      },
    };
  },
  watch: {
    postToBeEdited: {
      deep: true,
      immediate: true,
      handler(value) {
        this.newPost = value;
      },
    },
  },
  methods: {
    addPost() {
      const { newPost, postToBeEdited } = this;
      const apiURL = 'https://jsonplaceholder.typicode.com/posts';
      if (postToBeEdited && postToBeEdited.id) {
        fetch(`${apiURL}/${postToBeEdited.id}`, {
          method: 'PUT',
          body: JSON.stringify(newPost),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
          .then((response) => response.json())
          .then((data) => {
            console.log(data);
            if (data) {
              alert('Post updated successfully!');
              this.$emit('editPost');
            }
          })
          .catch((error) => alert(error));
      } else {
        fetch(apiURL, {
          method: 'POST',
          body: JSON.stringify(newPost),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
          .then((response) => response.json())
          .then((data) => {
            console.log(data);
            if (data) {
              alert('Post added successfully!');
              this.$emit('addNewPost', data);
            }
          })
          .catch((error) => alert(error));
      }
    },
  },
};
</script>

<style scoped>
.form {
  margin: 0 auto;
  padding: 24px;
  box-sizing: border-box;
  max-width: 400px;
}
</style>
