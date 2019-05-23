<!-- prettier-ignore -->
<template>
<div class="mdc-select">
  <input type="hidden" name="enhanced-select">
  <i class="mdc-select__dropdown-icon"></i>
  <div class="mdc-select__selected-text"></div>
  <div class="mdc-select__menu mdc-menu mdc-menu-surface">
    <ul class="mdc-list">
      <li class="mdc-list-item" role="option"
          v-for="option of options"
          :key="option.id"
          :data-value="option.id">
        {{ option.label }}
      </li>
    </ul>
  </div>
  <span class="mdc-floating-label">{{ label }}</span>
  <div class="mdc-line-ripple"></div>
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
      required: false
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },

  watch: {
    value: function(value) {
      if (!value && this.select) {
        this.select.selectedIndex = -1;
      }
    },
    disabled: function(value) {
      if (this.select) {
        this.select.disabled = value;
      }
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

    this.select.disabled = this.disabled;
    for (const [index, option] of this.options.entries()) {
      if (option.id === this.value) {
        this.select.selectedIndex = index;
        break;
      }
    }
  }
};
</script>

<style lang="scss">
$mdc-theme-primary: #1abc9c;

@import '@material/list/mdc-list';
@import '@material/menu-surface/mdc-menu-surface';
@import '@material/menu/mdc-menu';
@import '@material/select/mdc-select';

.mdc-list-item {
  white-space: nowrap;
  padding-right: 32px !important;
}
</style>
