<template>
  <div class="bg-sky-500 h-20 w-full flex justify-between px-20 ">
    <NuxtLink class="logo cursor-pointer" to="/">
    </NuxtLink>
    <div class="flex justify-center items-center">
      <NuxtLink class="btn-header" to="/">
        首页
      </NuxtLink>
      <div class="btn-header">
        个人图书馆
      </div>
      <div class="w-px bg-slate-300 h-8"></div>
      <div class="btn-header">
        登录
      </div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  width: 12rem;
  background: url("/logo.png") no-repeat;
  background-size: 100%;
  background-position: 50%;
}
</style>