<template>
  <div class="modal-container" @click.self="handleNo">
    <div class="modal-window">
      <p class="message">{{ message }}</p>
      <button class="yes" @click="handleYes">{{ yesText || defaultYesText }}</button>
      <button class="no" @click="handleNo">{{ noText || defaultNoText }}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'confirmation-popup',
  props: {
    message: {
      type: String,
      required: true,
    },
    yesText: {
      type: String,
    },
    noText: {
      type: String,
    },
    context: {
      required: true,
    },
  },
  data() {
    return {
      // Cannot use default prop value, since there's no `this` available there
      defaultYesText: this.$t('yes'),
      defaultNoText: this.$t('no'),
    };
  },
  methods: {
    handleYes() {
      this.$emit('yes', this.context);
    },
    handleNo() {
      this.$emit('no', this.context);
    },
  },
};
</script>

<style lang="scss" scoped>
.modal-container {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,0.9);
}

.modal-window {
  padding: 30px 15px;
  min-width: 300px;
  background: #eeeeee;
  display: grid;
  grid-template-columns: 50% 50%;
  align-items: center;
  justify-items: stretch;

  .message {
    grid-column-start: 1;
    grid-column-end: span 2;
  }

  .yes, .no {
    padding: 7px 5px;
  }
}
</style>
