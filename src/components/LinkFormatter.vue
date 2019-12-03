<template>
  <a
    v-if="isHttpType(link) && !isRelativeLink(link)"
    :href="getAbsoluteLink(link.type_options.subject)"
  >
    <slot />
    <span class="line line-right"></span>
    <span class="line line-top"></span>
    <span class="line line-left"></span>
    <span class="line line-bottom"></span>
  </a>
  <router-link
    v-else-if="isHttpType(link) && isRelativeLink(link)"
    :to="$routeToPath(`${link.type_options.subject}`)"
  >
    <slot />
    <span class="line line-right"></span>
    <span class="line line-top"></span>
    <span class="line line-left"></span>
    <span class="line line-bottom"></span>
  </router-link>
  <router-link v-else :to="getMenuLink(link)">
    <slot />
    <span class="line line-right"></span>
    <span class="line line-top"></span>
    <span class="line line-left"></span>
    <span class="line line-bottom"></span>
  </router-link>
</template>

<script>
import { getCoreMixin } from '../shared/mixin'
export default {
  name: 'LinkFormatter',
  mixins: [getCoreMixin('linkMixin')],
  props: {
    link: {
      type: Object,
      default: () => {},
    },
  },
}
</script>
