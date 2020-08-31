<template>
  <div>
    <h3>My Articles</h3>
    <ul>
      <li v-for="(article, index) in articles" v-bind:key="index">
        <div>
          <router-link
            v-bind:to="{ name: 'details', params: { articleId: article._id} }"
          >{{article.title}}</router-link>
        </div>
        <div>
          <router-link v-bind:to="{ name: 'edit', params: { articleId: article._id} }">Edit Article</router-link>
          <a href v-on:click.prevent="deleteArticle(article._id)">Delete Article</a>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "My-Articles",
  data: function() {
    return {
      articles: []
    };
  },

  components: {},

  methods: {
    // This method, when called and passed an id, sends a request to the api to have that article deleted
    deleteArticle: function(articleId) {
      this.$http
        .delete(`${process.env.VUE_APP_API_URL}articles/${articleId}`)
        .then(function() {
          // Once the response is received, reload the article list by calling get articles
          this.getArticles();
        });
    },

    // This function sends a request to the api to retreive all the articles, and then stores them in the article property of the data object
    getArticles: function() {
      this.$http
        .get(
          `${process.env.VUE_APP_API_URL}users/${localStorage.userId}/articles`
        )
        .then(function(data) {
          this.articles = data.body;
        });
    }
  },

  // This lifecycle hook executes whenever this component is created, and calls the function that retreives the articles from the api
  created: function() {
    this.getArticles();
  }
};
</script>

<style scoped>
</style>