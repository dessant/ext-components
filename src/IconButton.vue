<template>
  <button
    class="mdc-icon-button"
    :title="title"
    :disabled="disabled"
    @click="$emit('click')"
  >
    <slot>
      <img :src="src" class="mdc-icon-button__icon" />
    </slot>
  </button>
</template>

<script>
import {MDCRipple} from '@material/ripple';

export default {
  name: 'v-icon-button',

  props: {
    src: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean,
      default: false
    },
    ripple: {
      type: Boolean,
      default: true
    }
  },

  emits: ['click'],

  mounted: function () {
    if (this.ripple) {
      const iconButtonRipple = new MDCRipple(this.$el);
      iconButtonRipple.unbounded = true;
    }
  }
};
</script>

<style lang="scss">
@import '@material/icon-button/mdc-icon-button';

.mdc-icon-button::-moz-focus-inner {
  border: 0 !important;
}
</style>
