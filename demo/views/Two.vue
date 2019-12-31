<template>
  <div class="about">
    <x-condition-box v-model="condition" :fields="fields" />
  </div>
</template>

<script>
import XConditionBox from "@source/components/ConditionBox.vue";

export default {
  name: "about-page",
  components: {
    XConditionBox
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