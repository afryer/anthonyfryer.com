<template>
  <article class="hero">
    <nuxt-link :to="`/${this.heroArticle.slug}`">

    <div class="hero__image">
       <img v-if="featuredImage" v-lazy="heroImage.large.source_url" :data-srcset="heroImage.thumbnail.source_url + ' 500w,' + heroImage.medium.source_url + ' 900w'" alt="">
      </div>

      <div class="hero__date">
        <span v-html="shortTimestamp(heroArticle.date)"></span>&nbsp;–&nbsp;<span class="topic" v-for="topic in this.heroArticle._embedded['wp:term'][0]" :key="topic.id" v-html="topic.name"></span>
      </div>

      <div class="hero__meta">
        <h2 class="hero__title" v-html="this.heroArticle.title.rendered"></h2>
        <div class="hero_excerpt" v-html="this.heroArticle.excerpt.rendered"></div>
      </div>

    </nuxt-link>
  </article>
</template>

<script>
export default {
  props: {
    heroArticle: Object
  },
  data () {
    return {
      heroImage: null
    }
  },
  mixins: {
    shortTimestamp: Function
  },
  computed: {
    featuredImage () {
      let featuredImage = this.heroArticle._embedded['wp:featuredmedia']

      if (featuredImage) {
        this.heroImage = featuredImage[0].media_details.sizes
        return featuredImage[0].media_details.sizes.large || featuredImage[0].media_details.sizes.full || false
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  // @import '~assets/css/vars.scss';

  .hero {

    max-width: 900px;
    margin: 0 auto;
    overflow: hidden;

    &:hover {
      &::before {
        background-color: rgba(0, 0, 0, 0.25);
      }
    }



    a {
      // align-items: flex-end;
      // display: flex;

      &:hover {
        img {
          transform: scale(1.0125);
        }
      }
    }

    .date {
      text-transform: uppercase;


      .topic {
        & + .topic {
          margin-left: 8px;

          &::before {
            content: ', ';
            left: -7px;
            position: absolute;
          }
        }
      }

      a:hover {
      }
    }

    .hero__image {
      //min-height: 440px;
      width: 100%;
      overflow: hidden;

      img {
        object-fit: cover;
        display: block;
        height: 440px;
        max-width: 100%;
        transition: transform 0.6s;
        width: 100%;
      }
    }

  }
</style>
