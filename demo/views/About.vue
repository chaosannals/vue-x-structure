<template>
  <div class="about">
    <x-condition-structure v-model="condition" :fields="fields" />
  </div>
</template>

<script>
import XConditionStructure from "@source/widgets/Condition";

export default {
  name: "about-page",
  components: {
    XConditionStructure
  },
  data() {
    return {
      condition: null,
      fields: [
        {
          tag: "name",
          text: "名称",
          type: "string"
        },
        {
          tag: "count",
          text: "数量",
          type: "number"
        },
        {
          tag: "date",
          text: "日期",
          type: "date"
        }
      ]
    };
  }
};
</script>