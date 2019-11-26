<template>
  <div id="core-assets" class="mt40 mb40">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <div class="midsleek-typo-wrapper">
            <ul>
              <li>
                <h1 class="heading-section-midsleek">Heading 1</h1>
              </li>
              <li>
                <h2 class="heading-section-midsleek">Heading 2</h2>
              </li>
              <li>
                <h3 class="heading-section-midsleek">Heading 3</h3>
              </li>
              <li>
                <h4 class="heading-section-midsleek">Heading 4</h4>
              </li>
              <li>
                <h5 class="heading-section-midsleek">Heading 5</h5>
              </li>
              <li>
                <h6 class="heading-section-midsleek">Heading 6</h6>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Breadcrumbs from '../../components/Breadcrumbs'

export default {
  name: 'CoreAssets',
  components: {
    Breadcrumbs,
  },
  props: {
    page: {
      type: Object,
      default: () => {},
    },
    breadcrumbs: {
      type: Object,
      default: () => {},
    },
  },
  computed: {
    routes() {
      return [{ name: this.$t('Home'), route_link: '/' }]
    },
  },
}
</script>
