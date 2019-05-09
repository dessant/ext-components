<!-- prettier-ignore -->
<template>
<div class="mdc-switch" :class="switchClasses">
  <div class="mdc-switch__track"></div>
  <div class="mdc-switch__thumb-underlay">
    <div class="mdc-switch__thumb">
      <input type="checkbox" class="mdc-switch__native-control" role="switch"
          :checked="checked"
          :disabled="disabled"
          :id="`${id}__native`"
          @change="$emit('change', $event.target.checked)">
    </div>
  </div>
</div>
</template>

<script>
import {MDCSwitch} from '@material/switch';

export default {
  name: 'v-switch',

  computed: {
    switchClasses: function() {
      return {
        'mdc-switch--disabled': this.disabled
      };
    }
  },

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
    const mdcSwitch = new MDCSwitch(this.$el);
    this.$nextTick(function() {
      this.$parent.$emit('input-mounted', mdcSwitch);
    });
  }
};
</script>

<style lang="scss">
$mdc-theme-secondary: #1abc9c;

@import '@material/switch/mdc-switch';
</style>
