<template>
  <div class="mdc-text-field" :class="textFieldClasses">
    <textarea
      v-if="textarea"
      :id="`${id}__native`"
      ref="textarea"
      class="mdc-text-field__input"
      :value="value"
      @focus="onFocus"
      @blur="onBlur"
      @input="onInput"
      :placeholder="placeholder"
      :rows="rows"
      :cols="cols"
    >
    </textarea>

    <input
      v-if="!textarea"
      class="mdc-text-field__input"
      ref="input"
      :id="`${id}__native`"
      type="text"
      :value="value"
      @focus="onFocus"
      @blur="onBlur"
      @input="onInput"
      :placeholder="placeholder"
      :aria-label="placeholder"
    />

    <label
      v-if="label && !fullwidth && !outlined"
      :for="`${id}__native`"
      class="mdc-floating-label"
    >
      {{ label }}
    </label>

    <div v-if="!textarea && !outlined" class="mdc-line-ripple"></div>

    <div v-if="!textarea && outlined" class="mdc-notched-outline">
      <div class="mdc-notched-outline__leading"></div>
      <div class="mdc-notched-outline__notch">
        <label :for="`${id}__native`" class="mdc-floating-label">{{
          label
        }}</label>
      </div>
      <div class="mdc-notched-outline__trailing"></div>
    </div>
  </div>
</template>

<script>
import {MDCTextField} from '@material/textfield';

export default {
  name: 'v-textfield',

  props: {
    id: String,
    label: {
      type: String,
      required: false
    },
    placeholder: {
      type: String,
      required: false
    },
    value: {
      type: String,
      required: false
    },
    textarea: {
      type: Boolean,
      required: false
    },
    fullwidth: {
      type: Boolean,
      required: false
    },
    outlined: {
      type: Boolean,
      required: false
    },
    rows: {
      type: Number,
      required: false
    },
    cols: {
      type: Number,
      required: false
    }
  },

  emits: ['focus', 'blur', 'update:value'],

  methods: {
    onFocus: function () {
      this.$emit('focus');
    },

    onBlur: function () {
      this.$emit('blur');
    },

    onInput: function (event) {
      this.$emit('update:value', event.target.value);
    }
  },

  computed: {
    textFieldClasses: function () {
      return {
        'mdc-text-field--textarea': this.textarea,
        'mdc-text-field--fullwidth': this.fullwidth,
        'mdc-text-field--outlined': this.outlined
      };
    }
  },

  mounted: function () {
    this.textField = new MDCTextField(this.$el);
  }
};
</script>

<style lang="scss">
$mdc-theme-primary: #1abc9c;

@import '@material/textfield/mdc-text-field';
</style>
