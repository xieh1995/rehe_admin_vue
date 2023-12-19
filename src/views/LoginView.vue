<script lang="ts" setup>
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { useRouter } from 'vue-router'

const ruleFormRef = ref<FormInstance>()
const validate_event = ref(false)
const load = ref(false)

const username = (rule: any, value: any, callback: any) => {
  if (value === '') {
    callback(new Error('请输入账号'))
  } else {
    callback()
  }
}

const validatePass = (rule: any, value: any, callback: any) => {
  if (value === '') {
    callback(new Error('请输入密码'))
  } else {
    // if (ruleForm.username !== '') {
    //   if (!ruleFormRef.value) return
    //   ruleFormRef.value.validateField('checkPass', () => null)
    // }
    callback()
  }
}

const ruleForm = reactive({
  username: '',
  pass: ''
})

const rules = reactive<FormRules<typeof ruleForm>>({
  pass: [{ validator: validatePass, trigger: 'change' }],
  username: [{ validator: username, trigger: 'change' }]
})

const router = useRouter()
const submitForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.validate((valid) => {
    if (valid) {
      load.value = true
      setTimeout(() => {
        load.value = false
        router.push('/')
        console.log('submit!')
      }, 1000)
    } else {
      validate_event.value = true
      console.log('error submit!')
      return false
    }
  })
}
</script>

<template>
  <el-config-provider namespace="ep">
    <div class="login-index">
      <div class="login-left">
        <h1>Login</h1>
        <h3>email<code>a@abcdefg.com</code></h3>
      </div>
      <div class="login-right">
        <div style="width: 50%">
          <div style="display: flex; align-items: center; justify-content: space-between">
            <h1>登录</h1>
            <span>
              <span>没有账号？</span>
              <a href="javascript:void(0)"> 申请账号 </a>
            </span>
          </div>

          <el-form
            label-position="top"
            ref="ruleFormRef"
            :model="ruleForm"
            status-icon
            :rules="rules"
            label-width="120px"
            class="demo-ruleForm"
            size="large"
          >
            <el-form-item label="账号" prop="username">
              <el-input
                v-model="ruleForm.username"
                :validate-event="validate_event"
                autocomplete="off"
              />
            </el-form-item>
            <el-form-item label="密码" prop="pass" style="margin-top: 30px">
              <el-input
                v-model="ruleForm.pass"
                :validate-event="validate_event"
                style=""
                type="password"
                autocomplete="off"
              />
            </el-form-item>
            <el-form-item style="margin-top: 35px">
              <el-button
                type="primary"
                :loading="load"
                size="large"
                style="width: -webkit-fill-available; font-size: 1.2rem"
                @click="submitForm(ruleFormRef)"
                >登录</el-button
              >
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>
    <div class="login-footer">
      <code>2024@abcdefg</code>
    </div>
  </el-config-provider>
</template>

<style scoped>
.ep-form-item :deep(.ep-form-item__label) {
  font-size: 1.1rem;
}

.ep-form-item :deep(.ep-form-item__content .ep-form-item__error) {
  font-size: 1rem;
}

.ep-form-item :deep(.ep-form-item__content .ep-input__wrapper) {
  font-size: 1.1rem;
}

.login-index {
  display: flex;
  height: calc(100vh - var(--ep-footer-height));
}
.login-left {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  flex: 1;
  background: #eefbee;
}
.login-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  /* background: springgreen; */
}
.login-footer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: var(--ep-footer-height);
  background: #eaebeb;
}
</style>
