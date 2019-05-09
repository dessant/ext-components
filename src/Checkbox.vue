<!-- prettier-ignore -->
<template>
<div class="mdc-checkbox">
  <input type="checkbox" class="mdc-checkbox__native-control"
      :checked="checked"
      :disabled="disabled"
      :id="`${id}__native`"
      @change="$emit('change', $event.target.checked)">
  <div class="mdc-checkbox__background">
  <svg class="mdc-checkbox__checkmark" viewBox="0 0 24 24">
    <path class="mdc-checkbox__checkmark-path"
        fill="none"
        stroke="white"
        d="M1.73,12.91 8.1,19.28 22.79,4.59"/>
    </svg>
    <div class="mdc-checkbox__mixedmark"></div>
  </div>
</div>
</template>

<script>
import {MDCCheckbox} from '@material/checkbox';

export default {
  name: 'v-checkbox',

  model: {
    prop: 'checked',
    event: 'change'
  },

  props: {
    id: {
      type: String,
      required: true
    },
    checked: {
      type: Boolean
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },

  mounted: function() {
    const mdcCheckbox = new MDCCheckbox(this.$el);
    this.$nextTick(function() {
      this.$parent.$emit('input-mounted', mdcCheckbox);
    });
  }
};
</script>

<style lang="scss">
$mdc-theme-secondary: #1abc9c;

@import '@material/checkbox/mdc-checkbox';
</style>
