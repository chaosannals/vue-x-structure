<template>
  <div class="x-condition-structure">
    <x-structure-select style="width: 2em">
      <x-structure-option value="and">与</x-structure-option>
      <x-structure-option value="or">或</x-structure-option>
    </x-structure-select>
  </div>
</template>

<script>
import XStructureOption from "../widgets/Option.vue";
import XStructureSelect from "../widgets/Select.vue";

export default {
  name: "x-condition-structure",
  components: {
    XStructureOption,
    XStructureSelect
  },
  data() {
    return {};
  },
  mounted() {}
};
</script>

<style lang="scss" scoped>
.x-condition-structure {
}
</style>