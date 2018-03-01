<template>

  <div class="article-list">

    <article class="article-list__item" v-for="article in articles" :key="article.id">

      <nuxt-link :to="`/${article.slug}`" class="article-list__link">
          <div class="lazy" v-if="article._embedded['wp:featuredmedia']">
            <img class="article-list__image" v-lazy="article._embedded['wp:featuredmedia'][0].media_details.sizes.large.source_url" :data-srcset="article._embedded['wp:featuredmedia'][0].media_details.sizes.medium.source_url +  ' 400w'" />
          </div>
        <h3 class="article-list__title" v-html="article.title.rendered"></h3>
        <p class="article-list__date" v-html="shortTimestamp(article.date)"></p>
        <div class="article-list__teaser" v-html="article.excerpt.rendered"></div>
      </nuxt-link>
          <ul class="categories-meta">
      <li class="categories-meta__item" v-for="topic in $store.state.topics" v-if="topic.slug !== 'featured' && topic.count" :key="topic.id">
        <nuxt-link class="categories-meta__link" :to="`/topics/${topic.slug}`">
         <span v-html="topic.name"></span>
        </nuxt-link>
      </li>
    </ul>

    </article>

  </div>

</template>

<script>
export default {
  props: {
    articles: Array
  },
  mixins: {
    shortTimestamp: Function
  }
}
</script>

<style type="text/css scoped">

  .article-list {

    max-width: 900px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(1, 1fr);

    /* @media (min-width: 700px) {
      grid-template-columns: repeat(3, 1fr);
    } */

    //grid-gap: 30px;
  }

  .article-list__item {
    padding: 25px 0;

    @media (min-width:700px) {
      padding: 50px 0;
    }

    border-bottom: 1px solid #9f9f9f;
    overflow: hidden;
  }
  .article-list__link {

        text-decoration: none;


    &:hover {
      text-decoration: none;
    }
  }

  .article-list__title {
    font-weight: 400;
  }

  .article-list__image {
    border: 12px solid #fff;
    transition: transform 1.6s;

    &:hover {
      transform: scale(1.0125);
    }
  }

  .categories-meta__item {
    float: left;
    list-style: none;
    padding-right: 10px;
    &:after {
      content: ',';
    }
    &:last-child {
      &:after {
        content: '';
      }
    }
  }

</style>
