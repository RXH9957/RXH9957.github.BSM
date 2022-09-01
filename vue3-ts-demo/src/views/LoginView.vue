<template>
  <div class="login-box">
    <el-form
      ref="ruleFormRef"
      :model="ruleForm"
      status-icon
      :rules="rules"
      label-width="80px"
      class="demo-ruleForm"
    >
      <h2>后台管理系统</h2>
      <el-form-item label="账号：" prop="username">
        <el-input v-model="ruleForm.username" autocomplete="off" />
      </el-form-item>
      <el-form-item label="密码：" prop="password">
        <el-input
          v-model="ruleForm.password"
          type="password"
          autocomplete="off"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          class="loginBtn"
          type="primary"
          @click="submitForm(ruleFormRef)"
          >登录</el-button
        >
        <el-button class="loginBtn" @click="resetForm(ruleFormRef)"
          >重置</el-button
        >
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from 'vue'
import { LoginData } from '../type/login'
import type { FormInstance } from 'element-plus'
import { login } from '../request/api'
import { useRouter } from 'vue-router'
export default defineComponent({
  setup() {
    const data = reactive(new LoginData())
    const rules = {
      username: [
        {
          required: true,
          message: '请输入您的账号：',
          trigger: 'blur'
        },
        {
          min: 3,
          max: 10,
          message: '账号长度为3-10',
          trigger: 'blur'
        }
      ],
      password: [
        {
          required: true,
          message: '请输入您的密码',
          trigger: 'blur'
        },
        {
          min: 3,
          max: 10,
          message: '密码长度应为3-10',
          trigger: 'blur'
        }
      ]
    }
    //登录
    const ruleFormRef = ref<FormInstance>()
    const router = useRouter()
    const submitForm = (formEl: FormInstance | undefined) => {
      if (!formEl) return
      formEl.validate((valid: any) => {
        if (valid) {
          // console.log('submit!')
          // 保存token
          login(data.ruleForm).then((res) => {
            localStorage.setItem('token', res.data.taken)
            //跳转页面
            router.push('/')
          })
        } else {
          console.log('error submit!')
          return false
        }
      })
    }
    //重置
    const resetForm = () => {
      data.ruleForm.username = ''
      data.ruleForm.password = ''
    }
    return { ...toRefs(data), rules, resetForm, ruleFormRef, submitForm }
  }
})
</script>

<style lang="scss" scoped>
.login-box {
  width: 100%;
  height: 100%;
  background: url('../assets/background.png');
  overflow: hidden;
  text-align: center;
  background-repeat: no-repeat;
  background-size: 100%;

  .demo-ruleForm {
    width: 500px;
    margin: 200px auto;
    background-color: #fff;
    padding: 40px;
    border-radius: 20px;
  }
  .loginBtn {
    width: 48%;
  }
  h2 {
    margin-bottom: 30px;
  }
}
</style>
