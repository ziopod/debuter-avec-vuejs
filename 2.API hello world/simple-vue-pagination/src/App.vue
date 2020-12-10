<template>
  <div id="app">
    <h2>Posts WP</h2>
    <!--
    Des propriétés de composants sont définies (`:totalPages` et `:perPage`)
    et de valeurs sont transmissent (`totalPages` et `perPage`) à ce composant.

    Un écouteur d'évenement `v-on:` est spécifié pour surveiller `load-pages`, lorsque celui sera déclenché, il appelera la méthode `loadPage`. Notez que les paramètres associés à cet évenemnt (`page.id`) seront transmis à la méthode-->
    <Pagination
      :totalPages="totalPages"
      :perPage="perPage"
      v-on:load-page="loadPosts"
    />
 
    <div v-if="isLoading">
      <p>
        Chargement des posts…
      </p>
    </div>
    <div v-else>
      <!-- Point sémantique HTML : Si l'on veut présenter plus de contenu que des titres, il serait pertinent d'utiliser des élementent HTML `<article>` plutôt qu'une liste. -->
      <ul class="article">
        <!-- Un composant serait bienvenue pour représenter ces extraits d'articles. S'inspirer de la pagination pour tansmettre les informations nécessaire aux propriétes de ce futur composant.
          -->
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
/** WP API avec pagination **/
import Pagination from '@/components/Pagination.vue'
export default {
  name: 'SimpleVuePagination',
  components: {
    Pagination
  },
  data() {
    return {
      posts: [],
      isLoading: true,
      perPage: 25,
      totalPages: 0
    }
  },
  mounted() {
     this.loadPosts(1)
  },

  /**
   * Nous utilisons uen méthode, plutôt que d'éxéuter l'appel avec `mounted`
   * Ceci nous permet réemployer cette méthde à chaque clique sur un des 
   * bouton de la pagination
   */
  methods: {
    loadPosts(currentPage) {
      this.isLoading = true
      // N'hésitez pas à utiliser une autre source de données WP
      fetch('https://relikto.com/wp-json/wp/v2/posts?' + new URLSearchParams({
        context: 'view',
        page: currentPage,
        per_page: this.perPage,
      }), {
        method: 'GET',
      })
        .then(response => {
          this.totalPages = parseInt(response.headers.get('X-WP-TotalPages'))
          return response.json()
        })
        .then(json => {
          this.posts = json
          this.isLoading = false
        })
    }
  }
}
</script>

<style>
/**
 * Ce style peut éventuellement être placé dans le fichier
 * de style global `assets/styles.css`
 */
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
