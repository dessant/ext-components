<template>
  <div class="mdc-select" :class="classes">
    <div v-if="outlined" class="mdc-select__anchor">
      <i class="mdc-select__dropdown-icon"></i>
      <div class="mdc-select__selected-text"></div>
      <div class="mdc-notched-outline">
        <div class="mdc-notched-outline__leading"></div>
        <div class="mdc-notched-outline__notch">
          <label class="mdc-floating-label">{{ label }}</label>
        </div>
        <div class="mdc-notched-outline__trailing"></div>
      </div>
    </div>
    <div v-else class="mdc-select__anchor">
      <i class="mdc-select__dropdown-icon"></i>
      <div class="mdc-select__selected-text"></div>
      <span class="mdc-floating-label">{{ label }}</span>
      <div class="mdc-line-ripple"></div>
    </div>

    <div class="mdc-select__menu mdc-menu mdc-menu-surface">
      <ul class="mdc-list">
        <li
          class="mdc-list-item"
          v-for="option of options"
          :key="option.id"
          :data-value="option.id"
          :aria-selected="option.id === value"
        >
          {{ option.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import {MDCSelect} from '@material/select';

export default {
  name: 'v-select',

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
    outlined: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },

  emits: ['update:value'],

  data: function () {
    return {
      classes: {
        'mdc-select--outlined': this.outlined
      }
    };
  },

  watch: {
    value: function () {
      if (this.select) {
        this.setValue();
      }
    },
    disabled: function () {
      if (this.select) {
        this.setDisabled();
      }
    }
  },

  methods: {
    onChange: function () {
      this.$emit('update:value', this.select.value);
    },

    setDisabled: function () {
      this.select.disabled = this.disabled;
    },

    setValue: function () {
      if (!this.value) {
        this.select.selectedIndex = -1;
      } else {
        for (const [index, option] of this.options.entries()) {
          if (option.id === this.value) {
            this.select.selectedIndex = index;
            break;
          }
        }
      }
    }
  },

  mounted: function () {
    this.select = new MDCSelect(this.$el);
    this.select.listen('MDCSelect:change', this.onChange);

    this.setDisabled();
    this.setValue();
  }
};
</script>

<style lang="scss">
@import '@material/list/mdc-list';
@import '@material/menu-surface/mdc-menu-surface';
@import '@material/menu/mdc-menu';
@import '@material/select/mdc-select';

.mdc-list-item {
  white-space: nowrap;
  padding-right: 32px !important;
}
</style>
