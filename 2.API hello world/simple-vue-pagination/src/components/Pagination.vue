<template>
  <nav class="pagination">
    Pages : 
    <!--
    L'évenement natif `onClick` déclencheras l'évenement Vue `load-page` en
    lui associant le paramètre de l'identiiant de page à charger (`page.id`).
    cf. https://vuejs.org/v2/guide/events.html
    -->
    <button
      v-for="page in paginate" :key="page.id"
      @click="$emit('load-page', page.id)"
      class="button"
      >
      {{ page.name }}
    </button>
  </nav>  
</template>
<script>
export default {
  props: {
    totalPages: {
      type: Number,
      required: true
    },
    perPage: {
      type: Number,
      required: true
    }
  },
  computed: {
    paginate() {
      const links = []
      let count = 0
      while(count < Math.round(this.totalPages / this.perPage)) {
        count ++
        links.push({
          id: count,
          name: count
        })
      }
      return links
    }
  }  
}
</script>
<style>
/**
 * Tout comme les instructions javascript, le CSS reséervé
 * au composant doit être définie dans le composant.
 */
.pagination {
  /** 
   * Styliser la pagination ici
   * Jetez également un œil  à `src/assets/styles.css`
   */
}
</style>