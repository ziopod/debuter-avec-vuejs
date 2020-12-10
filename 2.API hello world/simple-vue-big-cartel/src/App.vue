<template>
  <div id="app">
    <h2>Boutique Big Cartel</h2>
    <div v-if="isLoading">
      <p>
        Chargement des produits…
      </p>
    </div>
    <div v-else>
      <article
        v-for="product in products"
        :key="product.id"
        class="product"
      >
        <h2>
          {{ product.name }}
          <span class="product-price">
            <small v-if="!product.on_sale">Bientôt en vente</small>
            <small v-else >{{ product.price }}€</small>
          </span>
        </h2>
        <p>{{ product.description }}</p>
        <div v-if="product.images[0]">
          <img
            :src="product.images[0].url"
            class="product-image"
          >
        </div>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  name: 'simple-vue-big-cartel',
  data() {
    return {
      products: [],
      isLoading: true,
    }
  },
  mounted() {
    let headers = new Headers();
    headers.append('Accept','application/vnd.api+json')
    headers.append('User-Agent', 'nom-utilisateur (email.de.contact@example.com)')

    const options = {
      method: 'GET',
      headers: headers
    }

    fetch('https://api.bigcartel.com/ziopod/products', options)
    .then(response => {
        return response.json()
     })
     .then((json) => {
       this.products = json
       this.isLoading = false
     })
     .catch(error => console.erro(error))
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
  margin: 0 auto;
  max-width: 35rem;
}
.product {
  padding: 1rem;
  border-bottom: 1px solid;
}
.product-image { max-width: 33%; }
.product-price { font-weight: normal; }
</style>
