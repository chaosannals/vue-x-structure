<template>
  <div class="x-condition-structure">
    <div class="action-pane">
      <x-structure-select class="type-select" v-model="type">
        <x-structure-option value="is">
          <div class="type-option">是</div>
        </x-structure-option>
        <x-structure-option value="not">
          <div class="type-option">非</div>
        </x-structure-option>
        <x-structure-option value="and">
          <div class="type-option">与</div>
        </x-structure-option>
        <x-structure-option value="or">
          <div class="type-option">或</div>
        </x-structure-option>
      </x-structure-select>
      <button v-if="isGroup" class="add-button" @click="onClickAdd">
        <span>+</span>
      </button>
    </div>
    <div v-if="isGroup" class="group">
      <div v-for="(c, i) in conditions" :key="c.id" class="condition-row">
        <x-condition-structure v-model="c.condition" :fields="fields" class="condition" />
        <button
          class="flow-button sub-button"
          :disabled="conditions.length <= 2"
          @click="onClickSub(i)"
        >
          <span>-</span>
        </button>
      </div>
    </div>
    <x-condition-item v-else v-model="condition" :fields="fields" class="item" />
  </div>
</template>

<script>
import uuidv1 from "uuid/v1";
import XStructureOption from "../widgets/Option.vue";
import XStructureSelect from "../widgets/Select.vue";
import XConditionItem from "../widgets/ConditionItem";

export default {
  name: "x-condition-structure",
  components: {
    XStructureOption,
    XStructureSelect,
    XConditionItem
  },
  data() {
    return {
      type: "is",
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
  computed: {
    condition: {
      get() {
        return this.value;
      },
      set(v) {
        this.$emit("input", v);
      }
    },
    isGroup() {
      return ["and", "or"].indexOf(this.type) >= 0;
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
  },
  mounted() {}
};
</script>

<style lang="scss" scoped>
.x-condition-structure {
  display: inline-flex;
  position: relative;
  overflow: visible;
  padding: 5px;
  box-shadow: 0 0 6px 0 #0003;

  .action-pane {
    display: inline-flex;
    flex-flow: row;
    align-items: center;
    justify-content: center;

    .add-button {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      box-sizing: border-box;
      width: 25px;
      height: 25px;
      margin: 5px 0;
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

  .type-select {
    width: 25px;
    height: 25px;
    border-radius: 50%;
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

  .group {
    position: relative;
    display: inline-flex;
    flex-flow: column;
    align-items: center;
    justify-content: space-around;
    flex-grow: 1;
    margin: 0 0 0 5px;

    .condition-row {
      position: relative;
      display: flex;
      align-items: stretch;
      justify-content: space-around;
      width: 100%;
      margin: 2px 0;

      .condition {
        flex-grow: 1;
      }
    }

    .flow-button {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 10px;
      margin: 0;
      padding: 0;
      color: white;
      border: 1px solid #e7e7e7;
      user-select: none;

      &.sub-button {
        background: #f00;

        &:enabled {
          background-color: #f00 !important;
        }
        &:disabled {
          background-color: #666 !important;
        }
      }
    }
  }

  .item {
    flex-grow: 1;
  }
}
</style>