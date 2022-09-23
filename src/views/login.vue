<template>
  <div class="login" @click="judgeCloseKeyboard">
    <div class="form-card" id="form-card">
      <div class="form-body">
        <el-form :model="form" ref="form" :rules="rules" @submit.native.prevent>
          <el-form-item prop="username">
            <KeyboardInput
              keyboardClass="login-username"   
              field="username"
              :value="form.username"
              :maxlength="10"
              clearable
              @inputChange="formItemChange"
              placeholder="用户名"
            ></KeyboardInput>
          </el-form-item>
          <button
            class="btn-login"
            :disabled="loginDisabled"
            @click.prevent="submit('form')"
          >
            {{ isSubmitting ? '登录中' : '登录' }}
          </button>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import  KeyboardInput  from '../components/keyboard-input.vue';
import { keyboardInputMixin } from '@/utils/mixins';
import { judgeCloseKeyboard } from '@/utils/keyboard-core';

export default {
  components: { KeyboardInput },
  mixins: [keyboardInputMixin], 
  data() {
    return {
      judgeCloseKeyboard,
      isSubmitting: false,
      form: {
        username: '',
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
        ],
      },
    };
  },
  computed: {
    loginDisabled() {
      return this.form.username === '' || this.isSubmitting;
    },
  },
  methods: {
    submit(formName) {
      this.$refs[formName].validate((valid, errFields) => {
        if (valid) {}
      });
    },
  },
};
</script>

<style lang="less" scoped>
@deep: ~'>>>';

.login {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  @{deep}.vue-touch-keyboard{
    width: 600px;
  }
}
.form-card {
  width: 550px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.8);
  padding: 40px 120px 80px 120px;
  box-sizing: border-box;
  .btn-login {
    width: 100%;
    height: 40px;
    background: #db3e5d;
    border-radius: 2px;
    border: none;
    color: rgba(255, 255, 255, 1);
    &:hover {
      cursor: pointer;
    }
    &:focus {
      outline: none;
    }
    &:disabled {
      cursor: not-allowed;
      background-color: rgba(219, 62, 93, 0.5);
    }
  }
}
@{deep}.el-form-item {
  margin-bottom: 30px;
  .el-input__inner {
    padding: 0 16px;
    font-size: 13px;
    border-radius: 2px;
    border-color: rgba(0, 0, 0, 0.15);
    &:hover,
    &:focus,
    &.is-active {
      border-color: #db3e5d;
    }
  }
}
</style>