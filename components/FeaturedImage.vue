<template>
  <div>
    <div class="featured-image lazy" v-if="featuredImage.source_url">
      <img v-lazy="featuredImage.source_url">
      </div>

    </div>
</template>

<script>
export default {
  props: {
    featuredImage: Object
  },
  data () {
    return {
    }
  },
  computed: {

  },
  watch: {

  }
}

</script>

<style lang="scss" scoped>
.featured-image {
  padding-bottom: 20px;
  @media (max-width: 700px) {
    padding-bottom: 100px;
  }
}
</style>

