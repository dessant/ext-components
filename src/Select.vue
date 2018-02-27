<template>
<div class="mdc-select"
    :class="selectClasses"
    :aria-disabled="disabled"
    role="listbox">
  <div class="mdc-select__surface" tabindex="0">
    <slot name="selection" :selection="value">
      <div class="mdc-select__label mdc-select__label--float-above">
        {{ label }}
      </div>
      <div class="mdc-select__selected-text"></div>
      <div class="mdc-select__bottom-line"></div>
    </slot>
  </div>
  <div class="mdc-menu mdc-select__menu">
    <ul class="mdc-list mdc-menu__items">
      <slot name="options" :selection="value" :options="options">
        <li class="mdc-list-item" role="option" tabindex="0"
            v-for="option in options"
            :key="option.id"
            :id="option.id"
            :aria-selected="value === option.id">
          {{ option.label }}
        </li>
      </slot>
    </ul>
  </div>
</div>
</template>

<script>
import {MDCSelect} from '@material/select';

export default {
  name: 'v-select',

  model: {
    prop: 'value',
    event: 'change'
  },

  props: {
    label: {
      type: String,
      default: ''
    },
    options: {
      required: true
    },
    value: {
      type: String,
      required: true
    },
    disabled: {
      type: Boolean,
      default: false
    },
    box: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    selectClasses: function() {
      return {
        'mdc-select--box': this.box
      };
    }
  },

  methods: {
    onChange: function() {
      this.$emit('change', this.select.value);
    }
  },

  mounted: function() {
    this.select = new MDCSelect(this.$el);
    this.select.listen('MDCSelect:change', this.onChange);
  }
};
</script>

<style lang="scss">
$mdc-theme-primary: #1abc9c;

@import '@material/select/mdc-select';
@import '@material/list/mdc-list';
@import '@material/menu/mdc-menu';
</style>
