<script setup>
const route = useRoute()
const id = route.query.id
const { data: notice } = await useFetch(`http://localhost:7001/api/notices/${id}`)
</script>
<template>
  <div>
    <div class="w-3/5 bg-white h-4/5 my-5 mx-auto py-5 shadow-md">
      <div class="m-5">当前位置：<NuxtLink to="/">首页</NuxtLink> > 最近通知</div>
      <div class="text-xl font-bold text-center my-10">{{ notice.data.Title }}</div>
      <div class="text-right my-10 mx-10 text-gray-500">发布时间：{{ formattedDate(notice.data.CreationDate) }}</div>
      <div class="px-20 leading-loose" v-html="notice.data.Content"></div>
    </div>
    <div class="w-3/5 bg-gray-700 h-10 mx-auto text-gray-400 flex justify-between px-32 leading-10 shadow-md">
      <div>版权所有：Library图书馆</div>
      <div>蜀ICP备XXXXXXXX号</div>
    </div>
  </div>
</template>