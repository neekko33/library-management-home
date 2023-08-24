<script setup>
import Cookies from 'js-cookie'
import { ElMessage } from 'element-plus';
const router = useRouter()

const state = reactive({
  dialogFormVisible: false,
  login: false,
  name: '',
  loginForm: {
    name: '',
    password: ''
  },
  signinForm: {
    name: '',
    password: '',
    confirmPassword: '',
    phone: '',
    email: ''
  },
  formLabelWidth: '80px',
  isLogin: true,
  dialogTitle: '读者登录',
  dialogWidth: '25%'
})

const name = Cookies.get('name')
if (name) {
  state.name = name
  state.login = true
}

watch(() => state.isLogin, (newValue, oldValue) => {
  state.dialogTitle = newValue ? '读者登录' : '读者注册'
  state.dialogWidth = newValue ? '25%' : '40%'
})

const checkForm = (data) => {
  let flag = true
  for (let key in data) {
    if (data[key].trim() === '') {
      ElMessage({
        message: '内容不能为空！',
        type: 'error'
      })
      flag = false
      break;
    }
  }
  return flag
}

const handleLogin = async () => {
  if (!checkForm(state.loginForm)) return
  const { data: result } = await useFetch('http://localhost:7001/api/readers/login', { method: 'POST', body: state.loginForm })
  Cookies.set('readerId', result.value.data.ReaderID, { expires: 3 })
  Cookies.set('name', result.value.data.Name, { expires: 3 })
  ElMessage({
    message: result.value.msg,
    type: 'success'
  })
  state.name = result.value.data.Name
  state.login = true
  state.dialogFormVisible = false
}

const handleLogout = () => {
  Cookies.remove('readerId')
  Cookies.remove('name')
  state.login = false
  state.name = ''
  router.push('/')
  ElMessage({
    message: '退出成功',
    type: 'success'
  })
}

</script>
<template>
  <div class="bg-sky-500 h-20 w-full flex justify-between px-20 ">
    <NuxtLink class="logo cursor-pointer" to="/">
    </NuxtLink>
    <div class="flex justify-center items-center">
      <NuxtLink class="btn-header" to="/">
        首页
      </NuxtLink>
      <NuxtLink class="btn-header" to="/personal">
        个人图书馆
      </NuxtLink>
      <div class="w-px bg-slate-300 h-8"></div>
      <template v-if="state.login">
        <el-dropdown trigger="click">
          <div class="btn-header">
            {{ state.name }}
          </div>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item @click="handleLogout">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </template>
      <div v-else class="btn-header" @click="state.dialogFormVisible = true">
        登录
      </div>
    </div>
    <ClientOnly>
      <el-dialog v-model="state.dialogFormVisible" :title="state.dialogTitle" :width="state.dialogWidth"
        @closed="state.isLogin = true">
        <el-form v-if="state.isLogin" :model="state.loginForm" label-position="top" class="px-10">
          <el-form-item label="用户名" :label-width="state.formLabelWidth">
            <el-input v-model="state.loginForm.name" autocomplete="off" />
          </el-form-item>
          <el-form-item label="密码" :label-width="state.formLabelWidth">
            <el-input v-model="state.loginForm.password" autocomplete="off" type="password" />
            <span class="w-full text-right text-sky-500 cursor-pointer hover:underline my-2"
              @click="state.isLogin = false">点击注册</span>
          </el-form-item>
          <div class="w-full h-10">
            <el-button class="w-full h-ful my-5" type="primary" @click="handleLogin">登录</el-button>
          </div>
        </el-form>
        <el-form v-else :model="state.signinForm" label-position="top" class="px-10">
          <el-form-item label="用户名" :label-width="state.formLabelWidth">
            <el-input v-model="state.signinForm.name" autocomplete="off" />
          </el-form-item>
          <el-form-item label="密码" :label-width="state.formLabelWidth">
            <el-input v-model="state.signinForm.password" autocomplete="off" type="password" />
          </el-form-item>
          <el-form-item label="确认密码" :label-width="state.formLabelWidth">
            <el-input v-model="state.signinForm.confirmPassword" autocomplete="off" type="password" />
          </el-form-item>
          <el-form-item label="手机号" :label-width="state.formLabelWidth">
            <el-input v-model="state.signinForm.phone" autocomplete="off" />
          </el-form-item>
          <el-form-item label="邮箱" :label-width="state.formLabelWidth">
            <el-input v-model="state.signinForm.email" autocomplete="off" />
            <span class="w-full text-right text-sky-500 cursor-pointer hover:underline my-2"
              @click="state.isLogin = true">返回登录</span>
          </el-form-item>
          <div class="w-full h-10 px-10">
            <el-button class="w-full h-ful my-5" type="primary">立即注册</el-button>
          </div>
        </el-form>
        <template #footer></template>
      </el-dialog>
    </ClientOnly>

  </div>
</template>

<style>
.logo {
  width: 12rem;
  background: url("/logo.png") no-repeat;
  background-size: 100%;
  background-position: 50%;
}
</style>