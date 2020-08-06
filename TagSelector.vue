<template>
  <div class="tag-input">
    <div class="tag" v-for="(tag, idx) in value" :key="`tag-${tag}-${idx}`">{{ tag }}</div>
    <input
      :id="id"
      :list="`${id}-datalist`"
      :placeholder="placeholder"
      v-model="userInput"
      @keyup="datalist = suggest(userInput)"
      @focus="datalist = suggest(userInput)"
      @blur="select"
      @keyup.backspace="handleBackspace"
      @keyup.enter.prevent="select"
      @keyup.188="select" />
    <datalist :id="`${id}-datalist`">
      <option v-for="entry in datalist" :key="`datalist-item-${entry}`">{{ entry }}</option>
    </datalist>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: String,
      required: false,
      default: 'tag-selector',
    },
    suggest: {
      type: Function,
      required: false,
      default: () => {},
    },
    value: {
      type: Array,
      required: true,
    },
    placeholder: {
      type: String,
      required: false,
      default: '',
    },
  },
  data() {
    return {
      userInput: '',
      datalist: [],
    };
  },
  methods: {
    select(ev) {
      let tag = this.userInput;
      if (/^\s*$/.test(tag)) return;
      tag = tag.replace(/(^[\s,]+|[\s,]+$)/gu, '');
      this.userInput = '';
      this.$emit('input', [...this.value, tag]);
      // do not propagate enter if we added something.
      // still propagates if we returned early due to empty input
      if (ev.keyCode === 13) ev.stopPropagation();
    },
    handleBackspace() {
      if (this.userInput === '') {
        if (this.value.length === 0) return;
        this.userInput = this.value[this.value.length-1];
        this.$emit('input', this.value.slice(0, -1));
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.tag-input {
  display: flex;

  div {
    flex-grow: 0;
    border: 1px solid black;
    border-radius: 5px;
    margin: 1px 3px;
    padding: 1px 2px;
    font-size: smaller;
  }
  input {
    flex-grow: 1;
  }
}
</style>
