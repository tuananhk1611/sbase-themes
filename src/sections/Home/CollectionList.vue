<template>
  <section class="section collection-list text-align-center">
    <div class="container">
      <h2 v-if="settings.heading" class="h3 mb40 heading-section-midsleek is-capitalized">
        {{ settings.heading }}
      </h2>
      <div v-if="collections.length > 0" class="row">
        <router-link
          v-for="(collection, index) in collections"
          :key="`${collection.id}-${index}`"
          ref="collection"
          class="col-4 col-md-4 collection-wrap link-initial"
          :to="$routeToCollection(collection)"
        >
          <div
            class="img-wrap"
            :ref="`collection-list-img-${index}`"
            :style="{
              fontSize: getImgHeight(`collection-list-img-${index}`),
              height: imgHeight,
            }"
          >
            <web-image
              :src="settings.blocks[index].image || collection.image"
              :alt="settings.blocks[index].alt_text"
              width="360"
              height="0"
            />
          </div>
          <p class="collection-name is-uppercase mt16">
            {{ collection.title }}
          </p>
        </router-link>
      </div>
      <div v-else class="row">
        <a
          v-for="(image, index) in defaultImages"
          :key="index"
          class="col-4 col-md-4 collection-wrap link-initial"
          target="_blank"
          :href="$themeEditorUrl"
        >
          <div class="img-wrap">
            <img v-lazy="image" alt />
          </div>
          <p class="collection-name is-uppercase mt8">
            {{ $t('Example collection title') }}
          </p>
        </a>
      </div>
    </div>
  </section>
</template>

<script>
import getDefaultImages from '../../utils/defaultImages'
export default {
  name: 'FeatureCollections',
  props: {
    collections: {
      type: Array,
      default: () => [],
    },
    settings: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      imgHeight: null,
    }
  },
  computed: {
    defaultImages() {
      return getDefaultImages('collectionList')
    },
  },
  mounted() {
    window.testRef = this.$refs
  },
  methods: {
    getImgHeight(el) {
      setTimeout(() => {
        if (this.$refs[el][0]) {
          this.imgHeight = `${this.$refs[el][0].clientWidth}px`
          return `${this.$refs[el][0].clientWidth}px`
        } else {
          return null
        }
      }, 300)
    },
  },
}
</script>
