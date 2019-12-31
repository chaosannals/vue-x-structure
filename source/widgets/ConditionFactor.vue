<template>
  <div class="x-condition-factor">
    <input v-if="type == 'number'" v-model="factor" type="number" class="number-input" />
    <input v-else-if="type == 'string'" v-model="factor" class="string-input" />
    <date-picker v-else-if="type == 'date'" v-model="factor" class="date-input"></date-picker>
    <input v-else v-model="factor" class="unknown-input" />
  </div>
</template>

<script>
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";

export default {
  name: "x-condition-factor",
  components: { DatePicker },
  props: {
    value: {
      default: null
    },
    type: {
      default: "number"
    }
  },
  computed: {
    factor: {
      get() {
        return this.value;
      },
      set(v) {
        this.$emit("input", v);
      }
    }
  }
};
</script>

<style lang="scss">
.x-condition-item {
  $height: 25px;

  .mx-datepicker {
    flex-grow: 1;
    height: $height;

    .mx-input-wrapper {
      height: $height;

      .mx-input {
        height: $height;
        border: 1px solid #e7e7e7;
        border-radius: 0;
      }
    }
  }
}
</style>

<style lang="scss" scoped>
.x-condition-factor {
  display: inline-flex;
  position: relative;

  .number-input {
    width: 100%;
    height: 100%;
    padding: 0 5px;
    box-sizing: border-box;
    border: 1px solid #e7e7e7;
  }

  .string-input {
    width: 100%;
    height: 100%;
    padding: 0 5px;
    box-sizing: border-box;
    border: 1px solid #e7e7e7;
  }

  .unknown-input {
    width: 100%;
    height: 100%;
    padding: 0 5px;
    box-sizing: border-box;
    border: 1px solid #e7e7e7;
  }
}
</style>