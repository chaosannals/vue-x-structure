<template>
  <div class="x-structure-select" tabindex="0" @focus="onFocus" @blur="onBlur">
    <div class="content">
      <component :is="content" :vnode="option"></component>
    </div>
    <input class="input" :name="name" :value="selection" />
    <div v-show="focus" ref="list" class="list">
      <slot @click="onOptionSelect"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "x-structure-select",
  data() {
    return {
      focus: false,
      observer: null,
      option: null,
      options: {},
      buffer: null,
      content: {
        functional: true,
        props: {
          vnode: Object
        },
        render: (h, ctx) => {
          return ctx.props.vnode;
        }
      }
    };
  },
  props: {
    name: String,
    value: {
      default: null
    }
  },
  computed: {
    selection: {
      get() {
        return this.value || this.buffer;
      },
      set(v) {
        this.buffer = v;
        this.$emit("input", v);
      }
    }
  },
  methods: {
    onFocus() {
      this.focus = true;
    },
    onBlur() {
      this.focus = false;
    },

    /**
     * 选项选中事件。
     *
     */
    onOptionSelect(component, option) {
      this.$el.blur();
      this.option = option;
      this.selection = component.value;
    },

    /**
     * 钩住选项选择事件。
     *
     */
    hookOptionSelect() {
      let newOptions = {};
      for (let option of this.$slots.default || []) {
        if (option.componentOptions.tag == "x-structure-option") {
          let id = option.componentInstance._uid;
          newOptions[id] = option;
          if (!this.options[id]) {
            this.options[id] = option;
            option.componentInstance.$on("x-select", component =>
              this.onOptionSelect(component, option)
            );
          }
        }
      }
      for (let id of Object.keys(this.options)) {
        if (!newOptions[id]) {
          this.options[id].componentInstance.$off("x-select");
        }
      }
      this.options = newOptions;
    }
  },
  mounted() {
    // 钩住
    this.observer = new MutationObserver(() => {
      this.hookOptionSelect();
    });
    this.observer.observe(this.$refs.list, {
      childList: true,
      subtree: true
    });
    this.hookOptionSelect();

    // 初始化值。
    for (let i of Object.keys(this.options)) {
      let option = this.options[i];
      if (option.componentInstance.value == this.value) {
        this.option = option;
      }
    }
  },
  beforeDestroy() {
    // 清除钩子
    this.observer.disconnect();
  }
};
</script>

<style lang="scss" scoped>
.x-structure-select {
  position: relative;
  overflow: visible;
  display: inline-block;
  box-sizing: border-box;
  border: 1px solid #e7e7e7;

  .content {
    display: flex;
    align-content: center;
    justify-content: center;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 1;
  }

  .input {
    visibility: hidden;
    width: 100%;
    height: 100%;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .list {
    position: absolute;
    left: 0;
    right: 0;
    z-index: 100;
    background: white;
    box-shadow: 0 0 6px 0 #0002;
  }
}
</style>