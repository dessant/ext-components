<template>
  <button
    class="mdc-button"
    :class="buttonClasses"
    :disabled="disabled"
    @click="$emit('click')"
  >
    <div class="mdc-button__ripple"></div>
    <span v-if="label" class="mdc-button__label">{{ label }}</span>
    <slot></slot>
  </button>
</template>

<script>
import {MDCRipple} from '@material/ripple';

export default {
  name: 'v-button',

  props: {
    label: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean,
      default: false
    },
    raised: {
      type: Boolean,
      default: false
    },
    unelevated: {
      type: Boolean,
      default: false
    },
    outlined: {
      type: Boolean,
      default: false
    },
    ripple: {
      type: Boolean,
      default: true
    }
  },

  emits: ['click'],

  computed: {
    buttonClasses: function () {
      return {
        'mdc-button--raised': this.raised,
        'mdc-button--unelevated': this.unelevated,
        'mdc-button--outlined': this.outlined
      };
    }
  },

  mounted: function () {
    if (this.ripple) {
      MDCRipple.attachTo(this.$el);
    }
  }
};
</script>

<style lang="scss">
@import '@material/button/mdc-button';
</style>
