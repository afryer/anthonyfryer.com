<template>
  <footer>
    <div v-if="meta">
      <nuxt-link to="/" exact><span v-html="meta.name"></span></nuxt-link>
      <div v-if="meta.description" v-html="meta.description"></div>
      <div>© {{ year }}</div>
    </div>
  </footer>
</template>

<script>
export default {
  computed: {
    meta () {
      return this.$store.state.meta
    },
    year () {
      return new Date().getFullYear()
    }
  }
}
</script>
