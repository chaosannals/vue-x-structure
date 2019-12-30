<template>
  <div class="x-structure-option" @click="onClick">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "x-structure-option",
  props: {
    value: {
      default: null
    }
  },
  methods: {
    onClick() {
      this.$emit("x-select", this);
    }
  }
};
</script>