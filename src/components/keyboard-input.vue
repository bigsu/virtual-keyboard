<template>
  <div class="input-keyboard">
    <el-input
      v-model="inputValue"
      :autofocus="autofocus"
      :class="inputClass"
      :suffix-icon="suffixIcon"
      :type="type"
      :show-password="showPassword"
      :rows="rows"
      :show-word-limit="showWordLimit"
      :disabled="disabled"
      :maxlength="maxlength"
      :clearable="clearable"
      :size="size"
      :placeholder="placeholder"
      @focus="focusInput($event)"
      @input="inputFun"
    >
      <template v-if="appendPort" slot="append">[1-65535]</template></el-input
    ><SimpleKeyboard
      :ref="keyboardClass"
      :keyboardClass="keyboardClass"
      @onChange="onChange"
      @onKeyPress="onKeyPress"
      :input="inputValue"
      :maxLength="maxlength"
    />
  </div>
</template>

<script>
import SimpleKeyboard from './simpleKeyboard.vue';

export default {
  name: 'keyboard-input',
  components: {
    SimpleKeyboard,
  },
  props: {
    keyboardClass: String,
    autofocus: Boolean,
    field: String,
    value: {
      default: '',
    },
    inputClass: String,
    type: {
      type: String,
      default: 'text',
    },
    showPassword: {
      type: Boolean,
      default: false,
    },
    rows: Number,
    showWordLimit: {
      default: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    maxlength: Number,
    clearable: {
      type: Boolean,
      default: true,
    },
    size: String,
    placeholder: String,
    appendPort: {
      type: Boolean,
      default: false,
    },
    autocomplete: {
      default: '',
    },
    suffixIcon: {
      default: '',
    },
  },
  data() {
    return {
      input: null,
      inputEle: null,
    };
  },
  computed: {
    inputValue: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit('inputChange', value, this.field);
      },
    },
  },
  methods: {
    inputChange() {
      this.$emit('inputChange');
    },
    inputFun() {
      this.$emit('input');
    },
    focusInput(e) {
      this.inputEle = e.srcElement;
      // 关闭所有keyboard
      let arr = document.querySelectorAll('.hg-theme-default');
      arr.forEach((ele) => {
        ele.style.visibility = 'hidden';
      });

      // 打开当前输入框的keyboard
      let currentKeyborad = this.$refs[this.keyboardClass];
      currentKeyborad.$el.style.visibility = 'visible';
      this.$emit('focus');
    },
    onChange(input) {
      this.inputValue = input;
      // 解决当输入框为密码输入框时，切换显示/隐藏密码，光标在开头问题，注意：element-ui版本号需为2.15.2及以上
      this.inputEle.focus();
    },
    onKeyPress(button) {
      // console.log('onKeyPress', button);
    },
  },
};
</script>

<style lang="less" scoped>
@deep: ~'>>>';
.input-keyboard {
  @{deep}.hg-theme-default {
    position: fixed;
    left: 50%;
    bottom: 20px;
    transform: translate(-50%);
    visibility: hidden;
    margin-top: 20px;
    z-index: 2000;
    // 中文文字选择框
    .hg-candidate-box {
      position: static;
      transform: translateY(0);
    }
    // 键盘
    .hg-rows {
    }
  }
  &.citc-search-input {
    .el-input {
      width: 100% !important;
    }
  }
  // 密码输入框显示密码图标效果
  @{deep}.el-input__inner[type='text'] + span .el-icon-view {
    color: red;
  }
}
</style>