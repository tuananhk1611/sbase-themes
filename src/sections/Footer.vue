<template>
  <div id="roller-section-footer" class="roller-section footer-section">
    <footer
      class="site-footer brdr-top-1 brdr-cl-whisper pt40"
      role="contentinfo"
    >
      <div class="container">
        <div class="row">
          <div class="col-6 col-sm-2">
            <!-- logo in footer -->
            <div class="logo mb32">
              <router-link
                to="/"
                class="logo-text items-center flex"
              >
                <img
                  v-if="getLogo"
                  class="fit-logo"
                  :src="getLogo"
                  :alt="$shop.name"
                  :style="`width: ${sectionSettings.width}px;`"
                />
                <div v-else ref="logo" class="items-center flex m0">
                  {{ $shop.name }}
                </div>
              </router-link>
            </div>
          </div>
        </div>
        <div class="row">
          <div
            v-for="(block, index) in blocks"
            :key="index"
            :class="`col-sm-${12 / (blocks.length || 1)} col-md-3`"
          >
            <div
              :class="[
                `mb40 ${
                  block.element_type === 'menu'
                    ? 'footer_menu'
                    : 'footer_content'
                }`,
              ]"
            >
              <h6
                class="h5 has-text-weight-medium is-capitalized has-text-gray-nero site-footer__header flex items-center mb16"
                @click.prevent="actives[index] = !actives[index]"
              >
                <span v-if="block.element_type !== 'page'" class="flex-grow">
                  {{
                    block.element_type === 'menu'
                      ? $getMenu(block.main_menu).title
                      : block.heading
                  }}
                </span>
                <span v-else class="flex-grow">
                  {{ page.title }}
                </span>
                <span
                  class="flex-basis material-icons icon-down-arrow"
                  :class="{ 'is-active': actives[index] }"
                >arrow_drop_down</span>
              </h6>
              <SCollapseTransition>
                <div v-show="actives[index]" class="toggle_content">
                  <ul
                    v-if="
                      block.element_type === 'menu' && $getMenu(block.main_menu)
                    "
                    class="list-style-none p0"
                  >
                    <li
                      v-for="item in $getMenu(block.main_menu).items"
                      :key="item.id"
                      class="footer_link my6"
                    >
                      <LinkFormatter :link="item">
                        {{ item.name }}
                      </LinkFormatter>
                    </li>
                  </ul>
                  <div
                    v-else-if="block.element_type === 'title'"
                    class="mb16"
                    v-html="block.text"
                  ></div>
                  <div v-else v-html="page.body_html"></div>
                </div>
              </SCollapseTransition>
            </div>
          </div>
        </div>
      </div>
      <div class="footer-second-content">
        <div class="container">
          <div class="row">
            <div class="col-sm-12">
              <div class="flex flex-wrap align-center pt24 footer-before">
                <div
                  v-if="sectionSettings.show_social_media_icons && sectionSettings.show_social_media_icons > 0"
                  class="footer-social mb24"
                >
                  <ul class="list-style-none m0 p0 flex">
                    <li v-if="sectionSettings.facebook_link" class="mr24">
                      <a
                        class="footer-social__icon footer-social__icon--fb"
                        :href="sectionSettings.facebook_link"
                        target="_blank"
                        title="Facebook"
                      ></a>
                    </li>
                    <li v-if="sectionSettings.twitter_link" class="mr24">
                      <a
                        class="footer-social__icon footer-social__icon--tw"
                        :href="sectionSettings.twitter_link"
                        target="_blank"
                        title="Twitter"
                      ></a>
                    </li>
                    <li v-if="sectionSettings.pinterest_link" class="mr24">
                      <a
                        class="footer-social__icon footer-social__icon--pin"
                        :href="sectionSettings.pinterest_link"
                        target="_blank"
                        title="Pinterest"
                      ></a>
                    </li>
                    <li v-if="sectionSettings.youtube_link" class="mr24">
                      <a
                        class="footer-social__icon footer-social__icon--you"
                        :href="sectionSettings.youtube_link"
                        target="_blank"
                        title="Youtube"
                      ></a>
                    </li>
                    <li v-if="sectionSettings.instagram_link" class="mr24">
                      <a
                        class="footer-social__icon footer-social__icon--ins"
                        :href="sectionSettings.instagram_link"
                        target="_blank"
                        title="Instagram"
                      ></a>
                    </li>
                    <li v-if="sectionSettings.blog_link">
                      <a
                        class="footer-social__icon footer-social__icon--blog"
                        :href="sectionSettings.blog_link"
                        target="_blank"
                        title="Blog"
                      ></a>
                    </li>
                  </ul>
                </div>
                <div class="footer-copyright mb24 text-align-right-sm">
                  <p class="display-7 is-capitalized m0">
                    © 2019 {{ $shop.name }}.
                    {{ sectionSettings.copyright_text }}
                  </p>
                </div>
              </div>
              <div class="flex flex-wrap align-center mb8 footer-after">
                <div class="footer-powered-by mb24" v-if="false">
                  <a
                    v-if="sectionSettings.show_powered_by !== false"
                    class="powered-by"
                    :href="poweredByLink"
                    target="_blank"
                  ></a>
                </div>
                <div class="footer-credits mb24 hidden-xs">
                  <ul
                    v-if="sectionSettings.show_payment_method_icons"
                    class="list-style-none m0 p0 flex"
                  >
                    <li
                      v-for="(payment, index) in paymentMethods"
                      :key="index"
                      :class="[
                      `footer-credits__icon footer-credits__icon--${payment}`,
                      { ml16: index !== 0 },
                    ]"
                    ></li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import { getCoreMixin } from '../shared/mixin'
import SCollapseTransition from '../utils/collapse'
import { FOOTER } from '../constants'
import { HEADER } from '../constants'
import LinkFormatter from '../components/LinkFormatter'
export default {
  name: 'Footer',
  components: { SCollapseTransition, LinkFormatter },
  mixins: [getCoreMixin('footerMixin')],
  data() {
    return {
      actives: {
        0: false,
        1: true,
        2: false,
        3: false,
      },
      paymentMethods: ['visa', 'master', 'amex', 'paypal', 'amazon'],
    }
  },
  computed: {
    sectionHeaderSettings() {
      return Object.assign({}, HEADER, this.$getFixedSettings('header'))
    },
    getLogo() {
      if (
        (this.isPreview && this.previewDevice === 'mobile') ||
        (!this.isPreview && this.$isMobile())
      ) {
        return this.sectionHeaderSettings.mobile_logo || this.sectionHeaderSettings.logo
      }

      return this.sectionHeaderSettings.logo || this.sectionHeaderSettings.mobile_logo
    },
    sectionSettings() {
      return Object.assign({}, FOOTER, this.$getFixedSettings('footer'))
    },
    blocks() {
      if (this.sectionSettings && this.sectionSettings.blocks) {
        return this.sectionSettings.blocks.filter(
          (block) =>
            (block.main_menu && block.main_menu !== 'none') ||
            block.text ||
            block.page
        )
      }

      return []
    },
    poweredByLink() {
      return `https://shopbase.com?utm_source=pm&utm_medium=poweredby&utm_name=storefooter&shopname=${
        this.$shop.name
      }`
    },
  },
  methods: {
    urlSrcSet(img) {
      // For local
      if (this.$shop.url && this.$shop.url.indexOf('http://localhost') !== -1) {
        let url = `${this.$resizeImage(img, '0', '180', '')} 1x, ${this.$resizeImage(
          img,
          '0',
          '180',
          '',
          'x2'
        )} 2x`
        url = url.replace('https://img', 'https://stag-img')
        return url
      }
      return `${this.$resizeImage(img, '0', '180', '')} 1x, ${this.$resizeImage(
        img,
        '0',
        '180',
        '',
        'x2'
      )} 2x`
    },
  },
}
</script>
