<template>
  <section
    v-if="settings.blocks.length"
    class="section slideshow-carousel special-block"
  >
    <div class="container-fluid">
      <Carousel
        id="gallery-carousel"
        ref="carousel"
        :per-page="1"
        :navigation-enabled="true"
        :mouse-drag="false"
        :touch-drag="false"
        :adjustable-height="true"
        :class="{ 'carousel--fade': settings.gallery_transition === 'fade' }"
        pagination-active-color="#1a1a1a"
        pagination-color="#e6e6e6"
        navigation-next-label="<i class='material-icons'>keyboard_arrow_right</i><div class='overlay'></div>"
        navigation-prev-label="<i class='material-icons'>keyboard_arrow_left</i><div class='overlay'></div>"
        :autoplay-timeout="
          (settings.change_slides_every ? settings.change_slides_every : 4) *
            1000
        "
        :autoplay="isAutoPlay"
        :loop="isAutoPlay"
        @pageChange="(page) => (currentSlide = page)"
      >
        <Slide
            v-for="(block, index) in settings.blocks"
            :key="index"
            :class="{ 'VueCarousel-slide-active': currentSlide === index }"
          >
            <div class="carousel-background mb24">
              <div class="overlay" :style="opacityLevel"></div>
              <web-image
                v-if="block.background_image"
                :src="block.background_image"
                width="2048"
                height="0"
                :alt="block.alt_text"
              />
              <img v-else v-lazy="bigImage" :alt="block.alt_text" />
            </div>
            <div
                class="text-wrap px15"
                :class="[
              textAlignment(block.text_alignment),
              textAnimation(settings.text_animation),
            ]"
              >
                <div class="container">
                  <div class="content" :class="textPosition(block.text_position)">
                    <p
                      v-if="block.preheading"
                      class="is-capitalized ls-2 mt0 mb0 midsleek-banner-preheading"
                      :style="stylePreheading"
                      v-html="block.preheading"
                    ></p>
                    <h2 v-if="block.heading" :style="styleHeading" class="h2 mt0 mb16 midsleek-banner-heading">
                      {{ block.heading }}
                    </h2>
                    <p
                      v-if="block.subheading"
                      class="ls-2 mt0 mb24 midsleek-banner-subheading"
                      v-html="block.subheading"
                    ></p>
                    <LinkFormatter
                      v-if="block.first_button_label && block.first_button_link"
                      :link="block.first_button_link"
                      class="btn carousel__first-link mb12"
                      :class="{
                  'btn-outline': !block.highlight_first_button_link,
                  'btn-primary banner-button':
                    block.highlight_first_button_link && highlightButtonType === 'primary',
                  'button-highlight banner-button':
                    block.highlight_first_button_link && highlightButtonType === 'subline',
                }"
                    >
                      {{ block.first_button_label }}
                    </LinkFormatter>
                    <LinkFormatter
                      v-if="block.second_button_label && block.second_button_link"
                      :link="block.second_button_link"
                      class="btn carousel__second-link"
                      :class="{
                  'btn-outline': !block.highlight_second_button_link,
                  'btn-primary banner-button':
                    block.highlight_second_button_link && highlightButtonType === 'primary',
                  'button-highlight banner-button':
                    block.highlight_second_button_link && highlightButtonType === 'subline',
                }"
                    >
                      {{ block.second_button_label }}
                    </LinkFormatter>
                  </div>
                </div>

              </div>
          </Slide>
      </Carousel>
    </div>
  </section>
</template>

<script>
import { Carousel, Slide } from '@jambonn/vue-carousel'
import { getCoreMixin } from '../../shared/mixin'
import LinkFormatter from '../../components/LinkFormatter'
import getDefaultImages from '../../utils/defaultImages'
export default {
  name: 'Slideshow',
  components: {
    Carousel,
    Slide,
    LinkFormatter,
  },
  mixins: [getCoreMixin('carouselTouchMixin')],
  props: {
    settings: {
      type: Object,
      default: () => {},
    },
    forceSettings: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      currentSlide: 0,
    }
  },
  computed: {
    bigImage() {
      return getDefaultImages('homepage')['bigImage']
    },
    opacityLevel() {
      return {
        opacity: this.settings.opacity
          ? parseFloat(this.settings.opacity) / 100
          : 0,
      }
    },
    isAutoPlay() {
      return !(typeof this.forceSettings.autoplay !== 'undefined')
    },
    styleHeading() {
      return {
        fontFamily: this.settings.heading_font_private ? 'Playfair Display' : 'inherit',
      }
    },
    stylePreheading() {
      return {
        fontFamily: this.settings.preheading_font_private ? 'Harmony' : 'inherit',
      }
    },
    highlightButtonType() {
      return this.settings.slideshow_button_highlight_type ? this.settings.slideshow_button_highlight_type : 'subline'
    },
  },
  watch: {
    forceSettings: {
      handler: function(val) {
        if (val) {
          this.$forceUpdate()
          this.$refs.carousel.goToPage(val.navigateTo)
        }
      },
      deep: true,
    },
  },
  methods: {
    textPosition(text_position) {
      switch (text_position) {
        case 'left':
          return 'pull-left'
        case 'right':
          return 'pull-right'
        case 'centre':
          return ''
        default:
          return 'text-wrap--left'
      }
    },
    textAlignment(text_alignment) {
      switch (text_alignment) {
        case 'left':
          return 'text-align-left'
        case 'centre':
          return 'text-align-center'
        case 'right':
          return 'text-align-right'
        default:
          return 'text-align-left'
      }
    },
    textAnimation(text_animation) {
      switch (text_animation) {
        case 'fade_in':
          return 'text-wrap--fade-in'
        case 'fade_up':
          return 'text-wrap--fade-up'
        case 'fade_down':
          return 'text-wrap--fade-down'
        default:
          return ''
      }
    },
  },
}
</script>
