<template>
  <div id="about-us-page" class="about-us-page">
    <div class="container">
      <div class="row">
        <div class="col-12 col-sm-4 figure-about">
          <img src="../../assets/images/about-midsleek.png" alt="about-midsleek">
        </div>
      </div>
      <div class="row">
        <div class="col-12">
          <div class="about-us-content">
            <h1 class="h1 heading-section-midsleek">
              Our Story
            </h1>
            <div class="row">
              <div class="col-12 col-sm-8 content-text-about">
                <p class="mb32">Like every other woman, I got married and got used to the busy life of being a wife,
                  a mom of two kids and a white-collar worker. Being fed up with all the housework and my career,
                  I was so stressed out during this time. Luckily, I gradually stepped out of the box
                  just by spending more time for myself and I realized that only comforting and pampering
                  myself can help balance my own life.
                </p>
                <p class="mb32">It comes to my attention that it is not only me having the trouble but also many of
                  my friends have these kinds of mid-life crisis. We all somehow were caught up with the
                  piled up work and forget how importance treasuring ourselves is. Consequently, we neglected
                  ourselves, depressed and lost the quality of our lives.
                </p>
                <p class="mb32">For that reason, I desire to set the inspiration to all my sleek mature girls out
                  there that we all have our own values and don’t forget to love yourself to brighten your life.
                  I decided to establish MidSleek - a brand for sleek women who were and being in the midlife period
                  but still passionate about fashion and beauty, and lust after shoes and bags.
                </p>
                <p class="mb24">
                  MidSleek was found with a mission to inspire women and to embrace their confidence in
                  fashionable presence. Put ourselves in the minds of our customers, we understand and
                  respond to customer needs. Our own products, always exploring, devising ways to bring
                  the best products in both quality and design. The most convenient way, through online
                  delivery systems nationwide delivery.
                </p>
                <p class="sign-text">
                  Enjoy Your Shopping With Us!
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <Newsletter
      v-if="newsletterSettings"
      :key="newsletterSettings.id"
      :settings="newsletterSettings.settings"
      :data-id="newsletterSettings.id"
    />
  </div>
</template>

<script>
import { getCoreMixin } from '../../shared/mixin'
import Breadcrumbs from '../../components/Breadcrumbs'
import BaseInput from '../../components/BaseInput'
import BaseTextarea from '../../components/BaseTextarea'
import Newsletter from '../../sections/Home/Newsletter'

export default {
  name: 'TemplatePageAboutUs',
  components: {
    Breadcrumbs,
    BaseInput,
    BaseTextarea,
    Newsletter
  },
  mixins: [getCoreMixin('contactUsMixin')],
  props: {
    page: {
      type: Object,
      default: () => {},
    },
    bodyHtml: {
      type: String,
      default: '',
    },
    routes: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    newsletterSettings() {
      const setting = this.$getPageSettings('homepage').filter((setting) => setting.type === 'newsletter')
      if (setting && setting.length > 0) return setting[0]
      return null
    },
  },
}
</script>
