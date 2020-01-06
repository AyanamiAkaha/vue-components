<template>
  <div class="numwidget">
    <button :class="decClasses" :disabled="value <= min" @click="dec"> - </button>
    {{ formatter(value) }}
    <button :class="incClasses" :disabled="value >= max" @click="inc"> + </button>
  </div>
</template>

<script>
export default {
  name: 'numeric-value-widget',
  props: {
    value: {
      type: Number,
      required: true,
    },
    increment: {
      type: Number,
      default: 1,
    },
    formatter: {
      type: Function,
      default: v => `${v}`,
    },
    min: {
      type: Number,
      default: Number.NEGATIVE_INFINITY,
    },
    max: {
      type: Number,
      default: Number.POSITIVE_INFINITY,
    },
    extraButtonClasses: { },
  },
  computed: {
    extraClassesArray() {
      if (!this.extraButtonClasses) return [];
      return this.extraButtonClasses instanceof Array
        ? this.extraButtonClasses
        : [this.extraButtonClasses];
    },
    decClasses() {
      return [
        'numwidget-dec-button',
        ...this.extraClassesArray,
      ];
    },
    incClasses() {
      return [
        'numwidget-inc-button',
        ...this.extraClassesArray,
      ];
    },
  },
  methods: {
    inc() {
      this.$emit('input', Math.min(this.max, this.value+this.increment));
    },
    dec() {
      this.$emit('input', Math.max(this.min, this.value-this.increment));
    },
  },
};
</script>

<style lang="scss" scoped>
.numwidget {
  display: grid;
  grid-template-columns: [dec] 2em [val] auto [inc] 2em;
}
.numwidget-inc-button {
  grid-column-start: inc;
}
.numwidget-dec-button {
  grid-column-start: dec;
}
</style>
