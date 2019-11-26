<template>
  <component
    :is="templateComponent"
    :routes="routes"
    :page="page"
    :body-html="bodyHtml"
  >
  </component>
</template>

<script>
import TemplateContactUs from '../templates/Page/ContactUs'
import TemplateDefault from '../templates/Page/Default'
import TemplateCoreAssets from '../templates/Page/CoreAssets'
import TemplateAboutUs from '../templates/Page/AboutUs'

export default {
  name: 'OnlineStorePage',
  components: {
    TemplateContactUs,
    TemplateDefault,
    TemplateCoreAssets,
    TemplateAboutUs
  },
  props: {
    page: {
      type: Object,
      default: () => {},
    },
    bodyHtml: {
      type: String,
      default: '',
    },
  },
  computed: {
    routes() {
      return [{ name: this.$t('Home'), route_link: '/' }]
    },
    templateComponent() {
      // Core assets
      if (typeof window !== 'undefined' && window.location.pathname === '/pages/core-assets' && typeof this.page.template_suffix === 'undefined') {
        return 'TemplateCoreAssets'
      }
      // About default
      if ((typeof window !== 'undefined' && window.location.pathname === '/pages/about-us' && typeof this.page.template_suffix === 'undefined') ||
        this.page.template_suffix === 'about') {
        return 'TemplateAboutUs'
      }
      if (this.page.template_suffix === 'contact') {
        return 'TemplateContactUs'
      }
      if (this.page.template_suffix === 'core') {
        return 'TemplateCoreAssets'
      }

      return 'TemplateDefault'
    },
  },
}
</script>
