<template>
  <div>
    <h1>Article Page</h1>
    <h2>{{article.title}}</h2>
    <p>{{article.body}}</p>
  </div>
</template>

<script>
export default {
  name: 'SingleArticle',
  data() {
    return {
      article: {},
    };
  },
  mounted() {
    console.log(this.$route.params);
    const { id } = this.$route.params;
    const apiURL = 'https://jsonplaceholder.typicode.com/posts';
    fetch(`${apiURL}/${id}`)
      .then((response) => response.json())
      .then((data) => {
        this.article = data;
      })
      .catch((error) => alert(error));
  },
};
</script>
