<template>
  <div class="articles">
    <article class="single-article">
      <FeaturedImage v-if="featuredImage" :expanded="expanded" :featuredImage="featuredImage"></FeaturedImage>
      <transition name="slide-fade">
        <div class="narrow">

          <div class="meta">
            <h1 class="title" v-html="page.title.rendered"></h1>
            <div class="details">
              <span>{{ longTimestamp(page.date) }}</span>
              <span class="separator">|</span>
              <nuxt-link class="author fancy" :to="`/authors/${author.slug}`">{{ author.name }}</nuxt-link>
            </div>
          </div>
          <div class="content" id="article-content" v-html="page.content.rendered"></div>
        </div>
      </transition>
    </article>
  </div>
</template>

<script>
import FeaturedImage from '~/components/FeaturedImage.vue'

export default {
  async asyncData ({ app, store, params }) {
    let page = await app.$axios.get(`${store.state.wordpressAPI}/wp/v2/pages?slug=${params.page}&_embed`)
    store.commit('setPage', page.data[0])
  },

  beforeMount () {
    // if (this.featuredImage.source_url) {
    //   let img = this.page._embedded['wp:featuredmedia'][0].media_details.sizes.thumbnail.source_url
    // }
  },

  components: {
    FeaturedImage
  },

  computed: {
    page () {
      return this.$store.state.page
    },
    author () {
      return this.$store.state.page._embedded.author[0]
    },
    featuredImage () {
      let featuredImage = this.$store.state.page._embedded['wp:featuredmedia']

      if (featuredImage) {
        return featuredImage[0].media_details.sizes.large || featuredImage[0].media_details.sizes.full || false
      } else {
        return { height: 0, width: 0 }
      }
    }
  },

  data () {
    return {
      expanded: false
    }
  },

  head () {
    return {
      title: `${this.page.title.rendered} | ${this.$store.state.meta.name}`,
      meta: [
        { description: this.page.excerpt.rendered }
      ]
    }
  },

  methods: {
  },

  mounted () {
  }
}
</script>

<style lang="scss" scoped>
  // @import '~assets/css/vars.scss';
  // @import '~assets/sass/styles.scss';

  // article {
  //   background-color: #efefef;
  //   display: flex;
  //   flex-direction: column;
  //   position: relative;
  //   height: 100%;

  //   &.page-enter-active .narrow {
  //     transition: transform 1s cubic-bezier(.11,.89,.31,.99), opacity 0.75s ease-out;
  //   }

  //   &.page-enter .narrow, .page-leave-to .narrow {
  //     transform: translateY(16px);
  //   }

  //   .narrow {
  //     background-color: #efefef;
  //     margin: 0 auto;
  //     max-width: 842px;
  //     min-height: calc(100vh - 80px - 96px - 200px);
  //     padding: 0 96px 96px 96px;
  //     position: relative;
  //     transition: min-height 1s, transform 1s;
  //     transform: translateY(0);
  //     width: 100%;

  //     &.expanded {
  //       min-height: 0;
  //     }

  //     @media (max-width: 900px) {
  //       margin-top: 0 !important;
  //       max-width: 100%;
  //       min-height: initial;
  //     }

  //     @media (max-width: 700px) {
  //       max-width: none;
  //       padding: 0 16px 16px 16px;
  //     }

  //     .expand-featured-image {
  //       background-color: transparent;
  //       border: 0;
  //       cursor: pointer;
  //       outline: 0;
  //       position: absolute;
  //       right: 32px;
  //       top: 32px;
  //       transition: 1s;
  //       z-index: 1;

  //       @media (max-width: 900px) {
  //         display: none;
  //       }

  //       &:hover {
  //         svg {
  //           opacity: 1;
  //         }
  //       }

  //       &.expanded {
  //         transform: rotate(180deg);
  //       }

  //       svg {
  //         fill: $primary;
  //         height: 24px;
  //         opacity: 0.7;
  //         transition: 0.1s;
  //         width: 24px;
  //       }
  //     }

  //     &.no-featured-image {
  //       margin: 0 auto;
  //     }
  //   }


  //   .meta {
  //     .title {
  //       font-size: 40px;
  //       line-height: 1;
  //       margin-bottom: 16px;
  //       margin-top: 0;
  //       padding-top: 64px;

  //       @media (max-width: 700px) {
  //         padding-top: 48px;
  //       }

  //       @media (max-width: 500px) {
  //         padding-top: 32px;
  //       }
  //     }

  //     .details {
  //       font-size: 0.8rem;

  //       .separator {
  //         padding: 0 0.5rem;
  //       }
  //     }
  //   }
  // }
</style>

<style lang="scss">
@import '../../node_modules/lightgallery.js/dist/css/lightgallery.css';
@import '~assets/css/vars.scss';

.lg-backdrop {
  background-color: #111;
}

#lg-counter {
  font-family: 'Roboto', sans-serif;
}

.lg-toolbar,
.lg-actions .lg-next,
.lg-actions .lg-prev,
.lg-outer .lg-thumb-outer,
.lg-outer .lg-toggle-thumb {
  background-color: #1a1a1a;
}

#lg-actual-size:after {
  content: "\E311";
}

#lg-zoom-in,
#lg-zoom-out {
  display: none;
}

.lg-outer .lg-thumb-item {
  border-radius: 2px;
}

.lg-outer .lg-thumb-item {
  border-color: #aaa;
}

.lg-outer .lg-thumb-item.active,
.lg-outer .lg-thumb-item:hover {
  border-color: #fff;
}
</style>

<style lang="scss">
// @import '~assets/css/vars.scss';

// .single-article {
//   .content {
//     border-top: 1px dotted lighten($primary, 20%);
//     padding-top: 32px;
//     margin-top: 32px;

//     .alignnone,
//     .size-full,
//     .wp-caption {
//       background-color: #fff;
//       padding: 16px;
//       display: block;
//       margin-bottom: 32px;

//       img {
//         display: block;
//         padding: 0;
//         margin-bottom: 16px;
//       }

//       &.alignnone {
//         max-width: 100%;
//       }

//       &.aligncenter {
//         margin-left: auto;
//         margin-right: auto;
//       }

//       &.alignleft {
//         float: left;
//         margin-right: 32px;
//         width: initial;
//       }

//       &.alignright {
//         float: right;
//         margin-left: 32px;
//         width: initial;
//       }
//     }

//     .wp-caption {
//       p {
//         margin-bottom: 0;

//         & + p {
//           margin-top: 16px;
//         }
//       }
//     }

//     p {
//       margin-bottom: 32px;
//       margin-top: 0;
//     }

//     a {
//       color: $accent;
//       position: relative;

//       &:hover {
//         color: $accent;
//       }

//       &::after {
//         background: rgba($accent, 0.5);
//         content: '';
//         height: 1px;
//         left: 0;
//         opacity: 0;
//         position: absolute;
//         top: 100%;
//         transform: translateY(-4px);
//         transition: height 0.1s, opacity 0.1s, transform 0.1s;
//         width: 100%;
//       }

//       &:hover,
//       &:focus {
//         &::after {
//           height: 4px;
//           opacity: 1;
//           transform: translateY(0px);
//         }
//       }
//     }

//     img {
//       height: auto;
//       max-width: 100%;
//     }

//     > *:first-child {
//       margin-top: 0;
//     }
//   }
// }
</style>
