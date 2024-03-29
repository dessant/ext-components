<template>
  <div
    :id="id"
    class="mdc-dialog"
    role="alertdialog"
    :aria-labelledby="labelId"
    :aria-describedby="descriptionId"
  >
    <div class="mdc-dialog__container">
      <div class="mdc-dialog__surface">
        <slot name="header">
          <!-- prettier-ignore -->
          <h2 :id="labelId" class="mdc-dialog__title header-title"><!--
         -->{{ title }}<!--
       --></h2>
        </slot>
        <div :id="descriptionId" class="mdc-dialog__content">
          <slot></slot>
        </div>
        <footer class="mdc-dialog__actions">
          <slot name="footer">
            <v-button
              v-if="cancelText"
              class="mdc-dialog__button"
              data-mdc-dialog-action="cancel"
              :label="cancelText"
            ></v-button>
            <v-button
              v-if="acceptText"
              class="mdc-dialog__button"
              data-mdc-dialog-action="accept"
              :label="acceptText"
            ></v-button>
          </slot>
        </footer>
      </div>
    </div>
    <div class="mdc-dialog__scrim"></div>
  </div>
</template>

<script>
import {MDCDialog} from '@material/dialog';

import Button from './Button';

export default {
  name: 'v-dialog',

  components: {
    [Button.name]: Button
  },

  props: {
    id: {
      type: String,
      required: true
    },
    title: {
      type: String,
      default: ''
    },
    acceptText: {
      type: String,
      default: ''
    },
    cancelText: {
      type: String,
      default: ''
    },
    showDialog: {
      type: Boolean,
      default: false
    }
  },

  emits: ['accept', 'cancel'],

  computed: {
    labelId: function () {
      return this.id ? `${this.id}__label` : false;
    },
    descriptionId: function () {
      return this.id ? `${this.id}__description` : false;
    }
  },

  mounted: function () {
    this.dialog = new MDCDialog(this.$el);
    this.dialog.listen('MDCDialog:closing', ev => {
      if (ev.detail.action === 'accept') {
        this.$emit('accept');
      } else if (['cancel', 'close'].includes(ev.detail.action)) {
        this.$emit('cancel');
      }
    });
  },

  watch: {
    showDialog: function (show) {
      if (show) {
        this.dialog.open();
      } else {
        this.dialog.close();
      }
    }
  }
};
</script>

<style lang="scss">
@import '@material/dialog/mdc-dialog';
</style>
