<template>
  <header>
    <div v-if="meta">
      <nuxt-link class="blog-title" to="/" exact>
        <h1 v-html="meta.name"></h1>
      </nuxt-link>
      <nav>
        <nuxt-link to="/" exact>Latest Articles</nuxt-link>
        <nuxt-link to="/topics">Topics</nuxt-link>
        <nuxt-link to="/pages/about">About</nuxt-link>
      </nav>
      <Search/>
    </div>
  </header>
</template>

<script>
import Search from '~/components/Search'

export default {
  components: {
    Search
  },

  computed: {
    meta () {
      return this.$store.state.meta
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~assets/sass/utilities/_variables.scss';

  header {
    left: 0;
  //  position: fixed;
    top: 0;
    width: 100%;
    z-index: 10;

    > div {
      align-items: center;
      border-bottom: 1px solid #d8d8d8;
      background-color: #fff;
      display: flex;
      height: 80px;
      margin: 0 auto;
      max-width: $containerWidth;
      padding: 0 32px;
      transition: padding-top 0.5s, padding-bottom 0.5s;

      @media (max-width: 700px) {
        height: 60px;
        padding: 16px;
      }
    }

    .blog-title {
      margin-right: 32px;

      @media (max-width: 700px) {
        margin-right: 16px;
      }
      h1 {
        margin-bottom: 0;
      }

    }

    nav {
      @media (max-width: 500px) {
        display: none;
      }
    }

    a {
      border-color: lighten($primary, 40%);
      color: #999;
      font-weight: 300;
      text-decoration: none;
      transition: color 0.1s, border-color 0.1s;

      &:hover {
        color: #555;
        border-color: #555;
      }

      &.nuxt-link-active:not(.blog-title) {
        padding-bottom: 4px;
        border-bottom: 2px solid;
      }

      & + a {
        margin-left: 32px;

        @media (max-width: 700px) {
          margin-left: 16px;
        }
      }
    }
  }

</style>

