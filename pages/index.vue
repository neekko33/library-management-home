<script setup>
const router = useRouter()
const { data: notices } = await useFetch('http://localhost:7001/api/notices/')
const handleSearch = (search) => {
  router.push('/search?s=' + search)
}
</script>

<template>
  <div>
    <SearchBar class="my-20" @search="handleSearch" />
    <div class="w-2/5 my-24 mx-auto ">
      <div class="text-xl font-bold">近期公告</div>
      <div>
        <NuxtLink v-for="(item, index) in notices.data" :key="index"
          class="my-4 hover:text-sky-500 cursor-pointer flex justify-between" :to="'/notice?id=' + item.NoticeID">
          <div class="w-4/5 truncate"><span class="icon"></span> {{ item.Title }}</div>
          <div class="w-1/5 truncate">[{{ formattedDate(item.CreationDate) }}]</div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<style scoped>
.icon {
  background: url(/list-icon.png) no-repeat;
  background-position: 50%;
  width: 15px;
  height: 10px;
  display: inline-block;
}
</style>
