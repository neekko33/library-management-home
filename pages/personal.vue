<script setup>
import Cookies from 'js-cookie'
const state = reactive({
  activeIndex: '1',
  readerId: 0,
  infoForm: {
    name: '',
    phone: '',
    email: '',
  },
  passwordForm: {
    password: '',
    newPassword: '',
    confirmPassword: ''
  },
  formLableWidth: '100px',
  borrowNow: null
})
const readerId = Cookies.get('readerId')
if (!readerId) {
  ElMessage({
    message: '请登录后再进行操作',
    type: 'error'
  })
  const router = useRouter()
  router.push('/')
}
state.readerId = readerId

const getUserInfo = async () => {
  const { data: result } = await useFetch(`http://localhost:7001/api/readers/${state.readerId}`)
  if (result.value) {
    state.infoForm.name = result.value.data.Name
    state.infoForm.phone = result.value.data.Phone
    state.infoForm.email = result.value.data.Email
  }
}

const getBorrowNow = async () => {
  const { data: result } = await useFetch(`http://localhost:7001/api/borrows/${state.readerId}`)
  if (result.value) {
    state.borrowNow = result.value.data
  }
}

const handleClick = event => {
  state.activeIndex = event.index
}

watch(() => state.activeIndex, async (newValue, oldValue) => {
  switch (newValue) {
    case '2':
      await getBorrowNow()
      break
  }
})

getUserInfo()

</script>
<template>
  <div class="common-layout w-3/5 h-full mx-auto my-8">
    <el-container>
      <el-aside width="200px" style="height: 80vh;">
        <el-menu default-active="1" class="el-menu-vertical-demo h-full">
          <el-menu-item index="1" @click="handleClick">
            <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
              <path fill="currentColor"
                d="M288 320a224 224 0 1 0 448 0 224 224 0 1 0-448 0zm544 608H160a32 32 0 0 1-32-32v-96a160 160 0 0 1 160-160h448a160 160 0 0 1 160 160v96a32 32 0 0 1-32 32z">
              </path>
            </svg>
            <span>个人信息</span>
          </el-menu-item>
          <el-menu-item index="2" @click="handleClick">
            <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
              <path fill="currentColor" d="M576 128v288l96-96 96 96V128h128v768H320V128h256zm-448 0h128v768H128V128z">
              </path>
            </svg>
            <span>当前借阅</span>
          </el-menu-item>
          <el-menu-item index="3" @click="handleClick">
            <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
              <path fill="currentColor"
                d="M704 192h160v736H160V192h160v64h384v-64zM288 512h448v-64H288v64zm0 256h448v-64H288v64zm96-576V96h256v96H384z">
              </path>
            </svg>
            <span>借阅记录</span>
          </el-menu-item>
          <el-menu-item index="4" @click="handleClick">
            <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
              <path fill="currentColor"
                d="M688 512a112 112 0 1 0 0 224h208v160H128V352h768v160H688zm32 160h-32a48 48 0 0 1 0-96h32a48 48 0 0 1 0 96zm-80-544 128 160H384l256-160z">
              </path>
            </svg>
            <span>罚款记录</span>
          </el-menu-item>
          <el-menu-item index="5" @click="handleClick">
            <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
              <path fill="currentColor"
                d="M764.416 254.72a351.68 351.68 0 0 1 86.336 149.184H960v192.064H850.752a351.68 351.68 0 0 1-86.336 149.312l54.72 94.72-166.272 96-54.592-94.72a352.64 352.64 0 0 1-172.48 0L371.136 936l-166.272-96 54.72-94.72a351.68 351.68 0 0 1-86.336-149.312H64v-192h109.248a351.68 351.68 0 0 1 86.336-149.312L204.8 160l166.208-96h.192l54.656 94.592a352.64 352.64 0 0 1 172.48 0L652.8 64h.128L819.2 160l-54.72 94.72zM704 499.968a192 192 0 1 0-384 0 192 192 0 0 0 384 0z">
              </path>
            </svg>
            <span>修改密码</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-main class="bg-white">
        <template v-if="state.activeIndex === '1'">
          <div class="font-bold text-xl">修改个人资料</div>
          <div class="border-dashed border-2 mt-5 py-14 px-28">
            <el-form :model="state.infoForm" label-position="top">
              <el-form-item label="用户名" :label-width="state.formLabelWidth">
                <el-input v-model="state.infoForm.name" autocomplete="off" disabled />
              </el-form-item>
              <el-form-item label="手机号" :label-width="state.formLabelWidth">
                <el-input v-model="state.infoForm.phone" autocomplete="off" />
              </el-form-item>
              <el-form-item label="邮箱" :label-width="state.formLabelWidth">
                <el-input v-model="state.infoForm.email" autocomplete="off" />
              </el-form-item>
              <div class="w-full h-10 flex items-center justify-center mt-10">
                <button class="w-3/5 h-full bg-sky-500 my-5 text-white hover:bg-sky-400 rounded-md">确认修改</button>
              </div>
            </el-form>
          </div>
        </template>
        <!-- TODO: 继续写当前借阅循环逻辑 -->
        <template v-else-if="state.activeIndex === '2'">
          <el-descriptions direction="vertical" :column="5" size="default" border>
            <el-descriptions-item label="序号">{{ index + 1 }}</el-descriptions-item>
            <el-descriptions-item label="图书名称">kooriookami</el-descriptions-item>
            <el-descriptions-item label="借阅时间">18100000000</el-descriptions-item>
            <el-descriptions-item label="应归还时间">Suzhou</el-descriptions-item>
            <el-descriptions-item label="是否逾期">
              <el-tag size="small">School</el-tag>
            </el-descriptions-item>
          </el-descriptions>
        </template>
        <template v-else-if="state.activeIndex === '3'">
          3
        </template>
        <template v-else-if="state.activeIndex === '4'">
          4
        </template>
        <template v-else>
          <div class="font-bold text-xl">修改密码</div>
          <div class="border-dashed border-2 mt-5 py-14 px-28">
            <el-form :model="state.passwordForm" label-position="top">
              <el-form-item label="原始密码" :label-width="state.formLabelWidth">
                <el-input v-model="state.passwordForm.password" autocomplete="off" type="password" />
              </el-form-item>
              <el-form-item label="新密码" :label-width="state.formLabelWidth">
                <el-input v-model="state.passwordForm.newPassword" autocomplete="off" type="password" />
              </el-form-item>
              <el-form-item label="重复密码" :label-width="state.formLabelWidth">
                <el-input v-model="state.infoForm.confirmPassword" autocomplete="off" type="password" />
              </el-form-item>
              <div class="w-full h-10 flex items-center justify-center mt-10">
                <button class="w-3/5 h-full bg-sky-500 my-5 text-white hover:bg-sky-400 rounded-md">确认修改</button>
              </div>
            </el-form>
          </div>
        </template>
      </el-main>
    </el-container>
  </div>
</template>

<style scoped>
svg {
  width: 20px;
  margin-right: 10px;
}
</style>