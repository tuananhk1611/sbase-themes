<template>
  <div
    :class="[
      'media-gallery-carousel',
      { 'media-gallery-carousel--loaded': imageLoaded },
    ]"
  >
    <Carousel
      id="media-gallery-carousel"
      ref="carousel"
      :per-page="1"
      :mouse-drag="false"
      :class="[
        {
          'carousel--fade':
            featuredProductSettings &&
            featuredProductSettings.gallery_transition
              ? featuredProductSettings.gallery_transition === 'fade'
              : '',
        },
        'VueCarousel-button',
      ]"
      :navigation-enabled="enableNavigation"
      :adjustable-height="true"
      :pagination-enabled="true"
      :pagination-padding="5"
      :pagination-active-color="'#1a1a1a'"
      :pagination-color="'#e6e6e6'"
      :loop="true"
      :navigate-to="navigateImgWithVariant()"
      navigation-next-label="<i class='material-icons'>keyboard_arrow_right</i>"
      navigation-prev-label="<i class='material-icons'>keyboard_arrow_left</i>"
      @pageChange="pageChange"
    >
      <Slide
        v-for="(image, index) in images"
        :key="image.src"
        class="product-gallery-image"
        :class="{ 'VueCarousel-slide-active': currentPage === index }"
      >
        <web-image
          v-show="hideImageAtIndex !== index"
          ref="images"
          :src="image.src"
          width="300"
          height="0"
          :alt="image.alt || productTitle | htmlDecode"
          :progressive="index === 0 || index === 1"
          @click="openOverlay"
          @load="onImageLoaded"
        />
        <div class="loader-balls w-100 h-100">
          <div class="loader-balls__wrapper flex justify-space-between items-center">
            <span class="loader-balls__item"></span>
            <span class="loader-balls__item"></span>
            <span class="loader-balls__item"></span>
          </div>
        </div>
      </Slide>
    </Carousel>

    <template v-if="images.length > 5">
      <Carousel
        ref="thumbnail_carousel"
        :per-page="5"
        class="thumbnail-carousel mt24"
        :navigation-enabled="true"
        :loop="true"
        navigation-next-label="<i class='material-icons'>keyboard_arrow_right</i>"
        navigation-prev-label="<i class='material-icons'>keyboard_arrow_left</i>"
      >
        <Slide
          v-for="(image, index) in images"
          :key="image.src"
          :class="[
            { 'thumbnail-carousel-slide--active': currentPage === index },
            'thumbnail-carousel-slide',
          ]"
        >
          <web-image
            v-show="hideImageAtIndex !== index"
            ref="images"
            :src="image.src"
            :alt="image.alt || productTitle | htmlDecode"
            width="180"
            height="0"
            :src-resolution="true"
            :square="true"
            @click="navigate(index, image.variant_ids)"
          />
          <div class="loader-balls w-100 h-100">
            <div class="loader-balls__wrapper flex justify-space-between items-center">
              <span class="loader-balls__item"></span>
              <span class="loader-balls__item"></span>
              <span class="loader-balls__item"></span>
            </div>
          </div>
        </Slide>
      </Carousel>
    </template>

    <ul
      v-else-if="images.length > 0"
      class="media-gallery-carousel__thumbs mt16 mb0 p0"
    >
      <li
        v-for="(image, key) in images"
        :key="image.src"
        class="media-gallery-carousel__thumb pointer"
        :class="{ 'active-thumb': key === currentPage }"
      >
        <span class="bg-cl-secondary d-block">
          <web-image
            ref="images"
            :src="image.src"
            width="180"
            height="0"
            :alt="image.alt || productTitle | htmlDecode"
            :src-resolution="true"
            :square="true"
            @click="navigate(key, image.variant_ids)"
          />
          <div class="loader-balls w-100 h-100">
            <div class="loader-balls__wrapper flex justify-space-between items-center">
              <span class="loader-balls__item"></span>
              <span class="loader-balls__item"></span>
              <span class="loader-balls__item"></span>
            </div>
          </div>
        </span>
      </li>
    </ul>
  </div>
</template>
<script>
import { Carousel, Slide } from '@jambonn/vue-carousel'
import { getCoreMixin } from '../../shared/mixin'

export default {
  name: 'ProductImagesCarousel',
  components: {
    Carousel,
    Slide,
  },
  mixins: [getCoreMixin('productImagesMixin')],
  props: {
    themeSettings: {
      type: Object,
      default: () => {},
    },
    featuredProductSettings: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      imageLoaded: false,
      currentPage: 0,
      hideImageAtIndex: null,
    }
  },
  computed: {
    enableNavigation() {
      if (
        this.featuredProductSettings &&
        this.featuredProductSettings.hasOwnProperty(
          'show_arrows_on_product_gallery'
        )
      ) {
        return this.featuredProductSettings.show_arrows_on_product_gallery
      }
      return true
    },
    /**
     * One thumbnail - one variant_id (1-1 relationship)
     * @return {boolean}
     */
    allVariantHasUniqueImage() {
      let listVariantIdsUnique = []
      let isAllVariantHasUniqueImage = true
      const imagesIsVariantId = this.images.filter(
        (image) => image.variant_ids.length === 1
      )
      if (imagesIsVariantId.length) {
        for (let image of imagesIsVariantId) {
          if (listVariantIdsUnique.indexOf(image.variant_ids[0]) < 0) {
            listVariantIdsUnique.push(image.variant_ids[0])
          } else {
            isAllVariantHasUniqueImage = false
            break
          }
        }
      } else {
        isAllVariantHasUniqueImage = false
      }
      return isAllVariantHasUniqueImage
    },
  },
  methods: {
    navigate(index, variantIds) {
      this.currentPage = index

      if (this.$refs.carousel) {
        this.$refs.carousel.goToPage(index)
      }

      if (
        this.themeSettings &&
        this.themeSettings.enable_select_variant_by_image &&
        variantIds &&
        variantIds.length === 1 &&
        this.allVariantHasUniqueImage
      ) {
        this.$emit('click-thumbnail', variantIds[0])
      }
    },
    openOverlay() {
      const currentSlide = this.$refs.carousel.currentPage
      this.$emit('toggle', currentSlide)
    },
    onImageLoaded() {
      this.imageLoaded = true
      if (this.$refs.hasOwnProperty('carousel')) {
        this.$refs.carousel.computeCarouselHeight()
      }
    },
    pageChange(index) {
      this.currentPage = index
      this.hideImageAtIndex = null

      if (
        this.$refs.thumbnail_carousel &&
        !this.$refs.thumbnail_carousel.$children[index].isActive
      ) {
        this.$refs.thumbnail_carousel.goToPage(Math.floor(index / 5))
      }
    },
  },
}
</script>
