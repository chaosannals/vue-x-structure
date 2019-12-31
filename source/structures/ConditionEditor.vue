<template>
  <div class="x-condition-editor"></div>
</template>

<script>
export default {
  name: "x-condition-editor",
  data() {
    return {
      selections: [],
      conditions: []
    };
  },
  props: {
    fields: {
      default: () => []
    }
  }
};
</script>

<style lang="scss" scoped>
.x-condition-editor {
  position: relative;
}
</style>