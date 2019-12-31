<template>
  <div class="x-condition-item">
    <x-structure-select v-model="field" class="field-select">
      <x-structure-option v-for="f in fields" :key="f.tag" :value="f">
        <div class="field-option" :title="f.text">{{f.text}}</div>
      </x-structure-option>
    </x-structure-select>
    <x-structure-select v-model="operator" class="operator-select">
      <x-structure-option v-for="o in operators" :key="o.tag" :value="o">
        <div class="operator-option" :title="o.text">{{o.text}}</div>
      </x-structure-option>
    </x-structure-select>
    <x-condition-factor v-model="factor" :type="field ? field.type : null" class="factor" />
    <button class="close-button" @click="onClose">
      <span>-</span>
    </button>
  </div>
</template>

<script>
import XStructureOption from "./Option.vue";
import XStructureSelect from "./Select.vue";
import XConditionFactor from "./ConditionFactor";

const OPERATORS = [
  {
    tag: "=",
    text: "等于",
    types: ["number", "string", "date"]
  },
  {
    tag: "!=",
    text: "不等于",
    types: ["number", "string"]
  },
  {
    tag: ">",
    text: "大于",
    types: ["number", "date", "datetime"]
  },
  {
    tag: ">=",
    text: "大于等于",
    types: ["number", "date", "datetime"]
  },
  {
    tag: "<",
    text: "小于",
    types: ["number", "date", "datetime"]
  },
  {
    tag: "<=",
    text: "小于等于",
    types: ["number", "date", "datetime"]
  },
  {
    tag: "like",
    text: "包含",
    types: ["string"]
  }
];

export default {
  name: "x-condition-item",
  components: {
    XStructureOption,
    XStructureSelect,
    XConditionFactor
  },
  data() {
    return {
      field: null,
      operator: null,
      factor: null
    };
  },
  props: {
    value: Object,
    fields: {
      default: () => []
    }
  },
  computed: {
    operators() {
      if (this.field) {
        let type = this.field.type;
        return OPERATORS.filter(i => i.types.indexOf(type) >= 0);
      }
      return [];
    }
  },
  watch: {
    /**
     * 当值改变时值。
     *
     */
    value(v) {
      if (v != this.value) {
        this.field = v.field;
        this.operator = v.operator;
        this.factor = v.factor;
      }
    },

    field() {
      this.setValue();
    },
    operator() {
      this.setValue();
    },
    factor() {
      this.setValue();
    }
  },
  methods: {
    onClose() {
      this.$emit("close");
    },
    /**
     * 设置值。
     *
     */
    setValue() {
      let v = {
        filed: this.field,
        operator: this.operator,
        factor: this.factor
      };
      this.$emit("input", v);
    }
  }
};
</script>

<style lang="scss" scoped>
.x-condition-item {
  position: relative;
  display: inline-flex;
  align-items: stretch;
  justify-content: center;
  margin: 2px 5px;
  padding: 5px;
  box-shadow: 0 0 6px 0 #0002;

  .field-select {
    width: 5em;
    margin: 0 5px;
  }

  .field-option {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    font-size: 12px;
  }

  .operator-select {
    width: 5em;
    margin: 0 5px;
  }

  .operator-option {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    font-size: 12px;
  }

  .factor {
    flex-grow: 1;
    margin: 0 5px;
  }

  .close-button {
    color: #fff;
    background: #f00;
    border: 1px solid #e7e7e7;
  }
}
</style>