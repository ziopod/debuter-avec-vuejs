# API Hello World

Conception d'une application [SPA](https://fr.wikipedia.org/wiki/Application_web_monopage) simple communiquant avec une [API](https://fr.wikipedia.org/wiki/Interface_de_programmation). Cette application sera basée sur le framework [Vue.js](https://vuejs.org/).

L'objectif de cet exercice est de découvrir l'usage de Vue.js sans librairies supplémentaires habituellement utiliser avec Vue.js. Mais cela ne vous empêche de jeter un œil à leurs usages : 
 
 - création d'une application Javascript avec Vue.js ;
 - gérer les données d'un composant Vue.js ;
 - appeler des données depuis une ressource API.

Requis : 
 - environnement [Nodejs](https://nodejs.org/en/download/) + framework [Vue CLI](https://cli.vuejs.org/guide/installation.html) ;
 - Lecture des composants des données Vue [VueDevtools](https://vuejs.org/v2/guide/installation.html#Vue-Devtools) ;

Une exemple complet de cet exercice est situé dans le repertoire `simple-vue`. Le repertoire `simple-vue-big-cartel` est une déclinaison avec une source de données provenant d'une boutique [Big Cartel](https://www.bigcartel.com/). Le repertoire `simple-vue-pagination` est une déclinaison de la source Worpdress avec une pagination sous forme de composant.

## Initialisation du projet

Une fois l'environnement installé, placez le pointeur du terminal dans le repertoire de votre projet : 

```
cd /chemin/vers/votre/projet/
```

Créer un nouveau projet Vue.js :

```
vue create simple-vue
```

Un questionnaire vous porpose des options, choissisez celles proposées par défaut.

Votre répertoire de travail devrais contenir le repertoire `simple-vue` contenant tout le nécessaire à la création d'une application Vue.js.

Pour tester votre installation, positionnez le curseur de votre terminal dans ce nouveau repertoire : 

```
cd simple-vue/
```

Puis lancez le serveur local de Vue.js :
```
npm run serve
```

Allez à l'adresse du serveur local indiqué (http://localhost:8081 par défaut), une page d'accueil doit s'afficher.

## Affichage de données

Nous allons travaillé dans le fichier `src/App.vue`. Vous pouvez supprimer le repertoire `src/assets` et `src/components` nous ne les utiliserons pas pour cet exercice. Nettoyons le fichier `App.vue`,conservez que le code suivant :

```
<template>
  <div id="app">
  </div>
</template>

<script>

export default {
  name: 'SimpleVue'
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

Observez le [marquage de template](https://vuejs.org/v2/api/#template) `<template>` spécifique de vue pour le HTML et le code d'affichage, le marqueur `<style>` natif de HTML pour la présentation CSS et également la marqueur `<script>` natif de HTML pour le code Javascript.

Chacun de ces marqueurs vont nous permettre de spécifier les divers élements de notre petite application : l'appel des données et le code logique avec le module javascript (`export`), le marquage HTML des données avec `<template>` et l'aspect avec le CSS; tout ceci constitue un [composant Vue](https://vuejs.org/v2/guide/components.html).

Ajoutons notre première méthode pour spécifier des donées Vue.js. Dans le bloc de code de `export default` déclarons la donnée `posts` : 
```
export default {
  name: 'Simple Vue'
  data() {
    return {
      posts: []
    }
  }
}
```

Observez les données de `posts` dans le VueDevtool. Vous y verrez un tableau vide.

Ajoutons une structure conditionnelle pour tenter d'afficher cette donnée. Dans la partie `<template>` ajoutez: 

```
<template>
  <div>
    <div v-if="posts.length">
      {{ posts }}
    </div>
    <div v-else>
      Chargement des posts…
    </div>
  </div>
</template>
```

Vous remarquerez `v-if` et `v-else` qui sont les arguments réservé de VueJS pour créer une structure conditionnelle. `v-if` permet d'évaluer directement du javascript (`posts.length` dans notre cas).

Temporairement ajoutons quelques données pour faire comprendre le fonctionnement : 
```
export default {
  data() {
    return {
      posts: [
        {
          id: 1,
          title: "Hello",
          body: "Hello world"
        },
        {
          id: 2,
          title: "Un autre article",
          body: "Contenu de cet article."
        }
      ]
    }
  }
}
```

Vous observerez les données bruts affichés dans la fenêtre de votre navigateur. Jetez un œil à VueDevtools, vous devriez y trouver les données que vous avez spécifiées pour `posts`.

Pour présenter ce contenu, ajoutons quelques élements HTML supplémentaire autours d'une boucle `for in`.

À la place de `{{ posts }}` écrivez : 
```
<ul>
    <li
      v-for="post in posts"
      :key="post.id"
    >
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </li>
</ul>
```

La boucle `for in` vas s'éxecuter auant de fois qu'ils y a d'entrée dans `posts`. Les données d'articles s'affichent sous forme de liste HTML.

## Création d'un appel API

Remplaçons maintenant nos données statiques par des données dynamiques provenant d'une source API.

Intialisons de nouveau notre donnée `posts` avec un tableau vide : 
```
data() {
  return {
    posts: []
  }
}
```

Ajoutons une fonction qui sera appelée lors du montage de l'instance de Vue avec [la fonction du cycle de vie Vue.js](https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram) [`mounted`](https://fr.vuejs.org/v2/api/index.html#mounted) : 

```
export default {
  name: 'SimpleVue',
  data() {
    return {
      posts: []
    }
  },
  mounted() {}
}
```

Lorsque l'instance de Vue.js sera montée (prête à l'usage), nous voulons appeler nos données API (source de données de démonstration de provenant de typicode.com) et attribuer le résultat à la donnée `posts`. Nous utiliserons [l'interface javascript native `fetch`](https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch) : 

```
mounted() {
  fetch('https://jsonplaceholder.typicode.com/posts')
    .then(response => {
      return response.json()
    })
    .then((result) => {
      this.posts = result
    })
    .catch(error => console.error(error))  
}
```

Vous pouvez maintenant observer la liste HTML peuplée avec les données provenant de l'appel API.

l'interface Fetch, fournis la méthode `fetch`, cette méthode est en charge d'appeller une ressource API et de lui transmettre des paramètres. Dans notre cas, nous nous contentons de préciser l'URL de la ressource, le paramètre d'action HTTP `GET` est transmis par défaut.

`fetch` peut être chainée avec la méthode `then`, celle-ci sert à déclencher [une fonction de rappel (callback)](https://developer.mozilla.org/fr/docs/Glossaire/Fonction_de_rappel). Une fois la rêquete de `fetch` terminée, le callback du premier `then` intercepte [la réponse de l'appel API `fetch`](https://developer.mozilla.org/fr/docs/Web/API/Response) et retourne le résultat au format JSON. 

Le deuxième `then` récupère la réponse au format JSON et l'attribut au paramètre de données Vue `posts`.

`catch` permet de capturer une erreur éventuelle et son callback l'afficheras alors dans la console de déboggage du navigateur.

## Une vrai source de données

Bien entendu, nous pouvons exploiter une vrai source de données API autre que la ressource de démonstration du placeholder de Typicode.

La plupart du temps, les services API réclamment que le logiciel à l'origine de l'appel API (nore application) s'identifie. Les méhodes pour s'identifier dépendent du service en question, il est donc indispensable de prendre connaissance de la documentation du service que vous voulez exploiter.

### Wordpress API 

Une source de données API simple à exploiter est via l'[API du CMS Wordpress](https://developer.wordpress.org/rest-api/). Worpdress embarque nativement un partage API de ses données, par défaut, l'accès ne nécesite pas d'authentification particulière.

Par exemple, voici comment afficher les 10 derniers article avec [la ressurce API posts de Wordpress](https://developer.wordpress.org/rest-api/reference/posts/) avec CURL :

```
curl https://relikto.com/wp-json/wp/v2/posts?context=view&per_page=10
```

Notez que nous passons certains paramètres `GET` directement dans l'url.

Mettons en place cet appel dans Vue.js avec la méthode native `fetch()` de Javascript:

```
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
```

Bien que les paramètres d'URL pourrait être passé avec ses paramètres `GET` dans l'URL comme ceci : 
~~~
`https://relikto.com/wp-json/wp/v2/posts?context=view&per_page=10`
~~~
, il est plus pratique d'utiliser [le constructueur `URLSearchParams()`](https://developer.mozilla.org/fr/docs/Web/API/URLSearchParams/URLSearchParams) pour passer les paramètres d'URL. 

Le résultat retourne est observable en l'affichant dans le template de cette manière :

```
<pre>{{ posts }}</pre>
```

ou en l'observant dans le VueDevTool.

Notez que le résultat diffère de Typicode.com, il faut adapté votre template en fonctions des données attendu : 

```
<ul>
    <li
      v-for="post in posts"
      :key="post.id"
    >
      <h2>{{ post.title.rendered }}</h2>
      <p>{{ post.excerpt.rendered }}</p>
    </li>
</ul>
```

Les données de titre et de l'extrait sont disponibles sous la propriété `rendered`, celle-ci contient les données au format HTML. Vous avez remarqué que Vue.js n'interprète pas les données HTML récupéré de cette manière (les balises HTML s'affiche sur la page du navigateur), c'est une mesure de sécurité.

Pour forçer Vue.js à interpréter ce données HTML, il faut utiliser la directive `v-html` de cette manière :

```
<ul>
    <li
      v-for="post in posts"
      :key="post.id"
    >
      <h2 v-html="post.title.rendered"></h2>
      <p v-html="post.body.rendered"></p>
    </li>
</ul>
```

### Big Cartel API

Prenons un autre exemple, la source de données API de Big Cartel. Sur leur documentation nous avons un chapitre qui nous détail comment mener à bien [une requête API sur leur service](https://developers.bigcartel.com/api/v1#making-api-calls).

Dans cette documentation, nous obtenons les informations suivantes : 
 - l'URL racine doit être `https://api.bigcartel.com`
 - un entête HTTP `Accept` doit etre fournis ;
 - un entête d'identification `User-Agent` doit être fournis ;
 - notre IP pourras être blacklisté si ces conditions en sont pas respéctées.

Ainsi un appel API auprès des services de Big Cartel devras ressembler à ça : 
```
curl --location --request GET 'https://api.bigcartel.com/ziopod/products' \
--header 'Accept: application/vnd.api+json' \
--header 'User-Agent: Votre-nom-utilisateur (email.de.contact@example.com)'
```

En Javascript, la requête pourras être formulée de cette manière avec `fetch` et l'[interface `Headers`](https://developer.mozilla.org/fr/docs/Web/API/Headers) : 

```
let headers = new Headers();
headers.append('Accept','application/vnd.api+json')
headers.append('User-Agent', 'Votre-nom-utilisateur (email.de.contact@example.com)')

const options = {
  method: 'GET',
  headers: headers
}
   
fetch('https://api.bigcartel.com/ziopod/products', options)
  .then(response => {
    return response.json()
  })
  .then((result) => {
    this.posts = result
  })
  .catch(error => console.erro(error))
```

Notez que nous passons l'objet d'options d'entêtes que nous avons crée est passé en second paramètre de `fetch`. Documentation : [ajouter des entête HTTP avec 'fetch`](https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch#En-t%C3%AAtes_Headers).

Comme pour l'API de Wordpress, il faudras ajuster l'affichage des données en fonction de ce que nou retourne l'API 

## Objectifs

Une fois mis en pratique cet exercice, mettez en place de manière autonaume les points suivants :

 - utilisez une source de données originale (WP, Big cartel, Contenfull, etc.) ;
 - améliorer le marquage sémantique HTML ;
 - personnalisé la présentation CSS ; 
 - utilisez un composant Vue.js original (cf. notes dans le source de `simple-vue-pagintion`)
