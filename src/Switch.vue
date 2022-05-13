<template>
  <div class="mdc-switch" :class="switchClasses">
    <div class="mdc-switch__track"></div>
    <div class="mdc-switch__thumb-underlay">
      <div class="mdc-switch__thumb">
        <input
          type="checkbox"
          class="mdc-switch__native-control"
          role="switch"
          :checked="checked"
          :disabled="disabled"
          :id="`${id}__native`"
          @change="$emit('update:checked', $event.target.checked)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import {MDCSwitch} from '@material/switch';

export default {
  name: 'v-switch',

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

  emits: ['update:checked', 'input-mounted'],

  computed: {
    switchClasses: function () {
      return {
        'mdc-switch--disabled': this.disabled
      };
    }
  },

  mounted: function () {
    const mdcSwitch = new MDCSwitch(this.$el);
    this.$nextTick(function () {
      this.$parent.emitter.emit('input-mounted', mdcSwitch);
    });
  }
};
</script>

<style lang="scss">
@import '@material/switch/mdc-switch';
</style>
