<template>
  <div class="mdc-form-field" :class="formFieldClasses">
    <slot></slot>
    <label :for="`${inputId}__native`">{{ label }}</label>
  </div>
</template>

<script>
import {MDCFormField} from '@material/form-field';
import mitt from 'mitt';

export default {
  name: 'v-form-field',

  props: {
    label: {
      type: String,
      required: true
    },
    inputId: {
      type: String,
      required: true
    },
    alignEnd: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    formFieldClasses: function () {
      return {
        'mdc-form-field--align-end': this.alignEnd
      };
    }
  },

  methods: {
    onInputMounted: function (input) {
      if (this.mdcFormField) {
        this.mdcFormField.input = input;
      }
    }
  },

  created: function () {
    this.emitter = mitt();
    this.emitter.on('input-mounted', this.onInputMounted);
  },

  mounted: function () {
    this.mdcFormField = new MDCFormField(this.$el);
  }
};
</script>

<style lang="scss">
@import '@material/form-field/mdc-form-field';

.mdc-form-field label {
  white-space: nowrap;
}
</style>
