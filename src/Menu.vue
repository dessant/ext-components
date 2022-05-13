<template>
  <div class="mdc-menu mdc-menu-surface">
    <ul
      class="mdc-list"
      role="menu"
      aria-hidden="true"
      aria-orientation="vertical"
      tabindex="-1"
    >
      <slot name="items" :items="items">
        <li
          class="mdc-list-item"
          role="menuitem"
          v-for="item of items"
          :key="item.id"
          :data-value="item.id"
        >
          <span class="mdc-list-item__text">{{ item.label }}</span>
        </li>
      </slot>
    </ul>
  </div>
</template>

<script>
import {MDCMenu, DefaultFocusState} from '@material/menu';
import {MDCRipple} from '@material/ripple';
import mitt from 'mitt';

export default {
  name: 'v-menu',

  props: {
    items: {
      required: true
    },
    focusItem: {
      type: String,
      default: ''
    },
    anchor: {
      type: String,
      default: ''
    },
    ripple: {
      type: Boolean,
      default: false
    }
  },

  emits: ['selected'],

  methods: {
    onOpen: function () {
      if (!this.menu.open) {
        this.menu.open = true;

        if (this.focusItem) {
          this.menu.setDefaultFocusState(DefaultFocusState.NONE);
          this.$el.querySelector(`li[data-value="${this.focusItem}"]`).focus();
        }
      }
    },

    onSelected: function (ev) {
      this.$emit('selected', ev.detail.item.dataset.value);
    }
  },

  created: function () {
    this.emitter = mitt();
    this.emitter.on('open', this.onOpen);
  },

  mounted: function () {
    this.menu = new MDCMenu(this.$el);
    if (this.anchor) {
      this.menu.setAnchorElement(document.querySelector(this.anchor));
    } else {
      this.menu.setFixedPosition(true);
    }

    this.menu.listen('MDCMenu:selected', this.onSelected);

    if (this.ripple) {
      for (const el of this.menu.items) {
        MDCRipple.attachTo(el);
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

.mdc-menu {
  z-index: 2147483647 !important;
}

.mdc-list-item {
  white-space: nowrap;
  padding-right: 32px !important;
}
</style>
