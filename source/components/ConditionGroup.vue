<template>
  <div class="x-condition-group">
    <div class="action-pane">
      <x-structure-select class="type-select" v-model="type">
        <x-structure-option value="and">
          <div class="type-option">与</div>
        </x-structure-option>
        <x-structure-option value="or">
          <div class="type-option">或</div>
        </x-structure-option>
      </x-structure-select>
      <button class="add-button" @click="onClickAdd">
        <span>+</span>
      </button>
    </div>
    <div class="condition-list">
      <div v-for="(c, i) in conditions" :key="c.id" class="condition-row">
        <x-condition-item
          v-model="c.condition"
          :fields="fields"
          class="condition"
          @close="onClickSub(i)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import uuidv1 from "uuid/v1";
import XConditionItem from "./ConditionItem";
import XStructureOption from "./Option.vue";
import XStructureSelect from "./Select.vue";

export default {
  name: "x-condition-group",
  components: {
    XConditionItem,
    XStructureOption,
    XStructureSelect
  },
  data() {
    return {
      type: "and",
      conditions: []
    };
  },
  props: {
    value: {
      default: null
    },
    fields: {
      default: () => []
    }
  },
  methods: {
    /**
     * 添加附加条件。
     *
     */
    onClickAdd() {
      let id = uuidv1();
      this.conditions.push({
        id: id,
        condition: {}
      });
    },

    /**
     * 删除附加条件。
     *
     */
    onClickSub(i) {
      this.conditions.splice(i, 1);
    }
  }
};
</script>

<style lang="scss" scoped>
.x-condition-group {
  position: relative;
  display: inline-flex;
  margin: 5px;
  box-shadow: 0 0 6px 0 #0002;

  .action-pane {
    display: inline-flex;
    flex-flow: column;
    align-items: center;
    justify-content: center;
    margin: 5px;

    .type-select {
      width: 25px;
      height: 25px;
      border-radius: 50%;
      outline: none;
    }

    .type-option {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
      font-size: 12px;
      user-select: none;
    }

    .add-button {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      box-sizing: border-box;
      width: 25px;
      height: 25px;
      margin: 5px 0 0 0;
      border: 1px solid #e7e7e7;
      border-radius: 50%;
      color: white;
      background: #37f;
      user-select: none;
      outline: none;

      &:hover {
        background: #7bf;
      }
      &:active {
        background: #16b;
      }
    }
  }

  .condition-list {
    position: relative;
    display: flex;
    flex-flow: column;
    flex-grow: 1;
  }
}
</style>