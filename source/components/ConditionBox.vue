<template>
  <div class="x-condition-box">
    <div class="action-bar">
      <button @click="addItem">添加单个条件</button>
      <button @click="addGroup">添加条件组合</button>
    </div>
    <div class="condition-area">
      <component v-for="c in conditions" :key="c.id" :is="c.component" :fields="fields"></component>
    </div>
  </div>
</template>

<script>
import uuidv1 from "uuid/v1";
import XConditionGroup from "./ConditionGroup";
import XConditionItem from "./ConditionItem";

export default {
  name: "x-condition-box",
  data() {
    return {
      conditions: []
    };
  },
  props: {
    fields: {
      default: () => []
    }
  },
  methods: {
    addGroup() {
      let id = uuidv1();
      this.conditions.push({
        id: id,
        component: XConditionGroup
      });
    },
    addItem() {
      let id = uuidv1();
      this.conditions.push({
        id: id,
        component: XConditionItem
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.x-condition-box {
  position: relative;
  display: inline-flex;
  flex-flow: column;
  box-shadow: 0 0 6px 0 #0002;

  .action-bar {
    position: relative;
    display: inline-flex;
    padding: 5px;
    margin-bottom: 5px;
    box-shadow: 0 0 6px 0 #0002;
  }

  .condition-area {
    position: relative;
    display: inline-flex;
    flex-flow: column;
    padding: 5px 2px;
    box-shadow: 0 0 6px 0 #0002;
  }
}
</style>