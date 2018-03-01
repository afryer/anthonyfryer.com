<template>
  <div class="home">
    <div class="banner">
      <div class="banner__info">
        <!-- <h1 class="banner__title" v-html="meta.name"></h1> -->
        <p class="banner__desc" v-html="meta.description"></p>
        <button v-scroll-to="'#articles'">
            down</button>
      </div>
    </div>

    <div class="articles" id="articles">
      <Hero :heroArticle="heroArticle" v-if="heroArticle"/>
      <ArticleList :articles="$store.state.articles"/>
      <InfiniteLoading v-if="indexInfiniteLoading.enabled" :on-infinite="moreArticles" ref="infiniteLoading"/>
    </div>

    <Strava />

  </div>

</template>

<script>
import ArticleList from '~/components/ArticleList'
import Hero from '~/components/Hero'
import InfiniteLoading from '~/components/InfiniteLoading'
import Strava from '~/components/Strava'

export default {
  async asyncData ({ app, store, params }) {
    if (!store.state.articles.length) {
      let articles = await app.$axios.get(`${store.state.wordpressAPI}/wp/v2/posts?orderby=date&per_page=10&categories_exclude=${store.state.featuredID}&_embed`)
      store.commit('setArticles', articles.data)
    }

    if (!store.state.featuredArticles.length) {
      let articles = await app.$axios.get(`${store.state.wordpressAPI}/wp/v2/posts?orderby=date&per_page=10&categories=${store.state.featuredID}&_embed`)
      store.commit('setFeaturedArticles', articles.data)
    }

    if (!store.state.topics) {
      let topics = await app.$axios.get(`${store.state.wordpressAPI}/wp/v2/categories?per_page=100`)
      store.commit('setTopics', topics.data)
    }
  },

  components: {
    ArticleList,
    Hero,
    InfiniteLoading,
    Strava
  },

  computed: {
    heroArticle () {
      return this.$store.state.articles[0]
    },
    indexInfiniteLoading () {
      return this.$store.state.indexInfiniteLoading
    },
    meta () {
      return this.$store.state.meta
    }
  },

  head () {
    return {
      title: `Home | ${this.$store.state.meta.name}`,
      meta: [
        { description: this.$store.state.meta.description }
      ]
    }
  },

  methods: {
    moreArticles () {
      this.indexInfiniteLoading.page++

      this.$axios.get(`${this.$store.state.wordpressAPI}/wp/v2/posts?orderby=date&per_page=10&categories_exclude=${this.$store.state.featuredID}&page=${this.indexInfiniteLoading.page}&_embed`)
        .then(response => {
          this.$store.commit('setArticles', response.data)
          this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded')
        })
        .catch(() => {
          this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete')
        })
    }
  }
}
</script>

<style lang="scss" scoped>
.banner {
  position: relative;
    display: table;
    width: 100%;
    height: 100vh;
    //margin-bottom: 5rem;
    text-align: center;
    background: url('~/assets/images/banner.jpg') no-repeat center center;
    background-size: cover;
    overflow: hidden;
}

.banner__info {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
}

.banner__title {
  font-size: 6rem;
  color: #fff;
}

.banner__desc {
  font-size: 4rem;
  color: #fff;
}

button {
  animation: bounce 4s 2s infinite;
}

/* Used to bounce .scroll-down on home page */
@-webkit-keyframes bounce {
    0%, 10%, 25%, 40%, 50% {
        -webkit-transform: translateY(0);
                transform: translateY(0);
    }
    20% {
        -webkit-transform: translateY(-10px);
                transform: translateY(-10px);
    }
    30% {
        -webkit-transform: translateY(-5px);
                transform: translateY(-5px);
    }
}
@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        -webkit-transform: translateY(0);
                transform: translateY(0);
    }
    40% {
        -webkit-transform: translateY(-10px);
                transform: translateY(-10px);
    }
    60% {
        -webkit-transform: translateY(-5px);
                transform: translateY(-5px);
    }
}

</style>
