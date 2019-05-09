<!-- prettier-ignore -->
<template>
<div role="progressbar" class="mdc-linear-progress">
  <div class="mdc-linear-progress__buffering-dots"></div>
  <div class="mdc-linear-progress__buffer"></div>
  <div class="mdc-linear-progress__bar mdc-linear-progress__primary-bar">
    <span class="mdc-linear-progress__bar-inner"></span>
  </div>
  <div class="mdc-linear-progress__bar mdc-linear-progress__secondary-bar">
    <span class="mdc-linear-progress__bar-inner"></span>
  </div>
</div>
</template>

<script>
import {MDCLinearProgress} from '@material/linear-progress';

export default {
  name: 'v-linear-progress',
  props: {
    determinate: {
      type: Boolean,
      default: true
    },
    reverse: {
      type: Boolean,
      default: false
    },
    progress: {
      type: Number,
      default: 0
    },
    buffer: {
      type: Number,
      default: 1
    }
  },

  mounted: function() {
    this.bar = new MDCLinearProgress(this.$el);
    this.bar.determinate = this.determinate;
    this.bar.reverse = this.reverse;
    this.bar.progress = this.progress;
    this.bar.buffer = this.buffer;

    this.bar.open();
  },

  watch: {
    determinate: function(value) {
      if (this.bar) {
        this.bar.determinate = value;
      }
    },
    reverse: function(value) {
      if (this.bar) {
        this.bar.reverse = value;
      }
    },
    progress: function(value) {
      if (this.bar) {
        this.bar.progress = value;
      }
    },
    buffer: function(value) {
      if (this.bar) {
        this.bar.buffer = value;
      }
    }
  }
};
</script>

<style lang="scss">
$mdc-theme-primary: #1abc9c;

@import '@material/linear-progress/mdc-linear-progress';
</style>
