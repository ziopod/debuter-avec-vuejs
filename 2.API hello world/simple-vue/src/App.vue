<template>
  <div id="app">
    <h2>Posts WP</h2>
    <div v-if="! posts.length">
      <p>
        Chargement des posts…
      </p>
    </div>
    <div v-else>
      <ul>
        <li v-for="post in posts" :key="post.id">
          <a :href="post.link">
            <h3 v-html="post.title.rendered"></h3>
          </a>
          <div v-html="post.excerpt.rendered"></div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
/** WP API **/
export default {
  name: 'SimpleVue',
  data() {
    return {
      posts: [],
    }
  },
  mounted() {
    fetch('https://relikto.com/wp-json/wp/v2/posts?' + new URLSearchParams({
      context: 'view',
      per_page: 10,
    }))
      .then(response => {
        return response.json()
      })
      .then(json => {
        this.posts = json
      })
     .catch(error => console.error(error))
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  max-width: 35rem;
  margin: 0 auto;
}
.article {
  list-style: none;
  padding-left: 0;
}
</style>
