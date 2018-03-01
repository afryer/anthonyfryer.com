<template>
  <div class="article">
    <FeaturedImage v-if="featuredImage" :featuredImage="featuredImage"></FeaturedImage>
  <article class="single-article">



    <transition name="slide-fade">
      <div class="narrow">

        <div class="meta">
          <h1 class="title" v-html="article.title.rendered"></h1>
          <div class="details">
            <span>{{ longTimestamp(article.date) }}</span>
            <span class="separator">|</span>
            <nuxt-link class="author fancy" :to="`/authors/${author.slug}`">{{ author.name }}</nuxt-link>
          </div>
        </div>
        <div class="content" id="article-content" v-html="article.content.rendered"></div>
        <div class="comments">
          <div class="loading" v-if="!disqusReady">
            <Spinner1></Spinner1>
            <span>Loading comments</span>
          </div>
          <div class="disqus" :class="{ ready: disqusReady }">
            <no-ssr>
              <lazy-component>
                <VueDisqus shortname="anthonyfryer" :identifier="article.slug" @ready="disqusReady = true"></VueDisqus>
              </lazy-component>
            </no-ssr>
          </div>
        </div>
      </div>
    </transition>
  </article>
  </div>
</template>

<script>
import VueDisqus from 'vue-disqus/VueDisqus.vue'
import FeaturedImage from '~/components/FeaturedImage.vue'
import Spinner1 from '~/components/Spinner1'

export default {
  async asyncData ({ app, store, params }) {
    let article = await app.$axios.get(`${store.state.wordpressAPI}/wp/v2/posts?slug=${params.article}&_embed`)
    store.commit('setArticle', article.data[0])
  },
  beforeMount () {
  },
  mixins: {
    longTimestamp: Function
  },
  components: {
    Spinner1,
    VueDisqus,
    FeaturedImage
  },
  computed: {
    article () {
      return this.$store.state.article
    },
    author () {
      return this.$store.state.article._embedded.author[0]
    },
    featuredImage () {
      let featuredImage = this.$store.state.article._embedded['wp:featuredmedia']
      if (featuredImage) {
        return featuredImage[0].media_details.sizes.large || featuredImage[0].media_details.sizes.full || false
      } else {
        return { height: 0, width: 0 }
      }
    }
  },
  data () {
    return {
      disqusReady: false
    }
  },
  head () {
    return {
      title: `${this.article.title.rendered} | ${this.$store.state.meta.name}`,
      meta: [
        { description: this.article.excerpt.rendered }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
