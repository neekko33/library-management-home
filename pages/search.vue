<script setup>
const page = ref(1)
const { data: books } = await useFetch('http://localhost:7001/api/books/')
const { data: categories } = await useFetch('http://localhost:7001/api/categories/all')
const handlePageChange = () => {

}
</script>
<template>
  <div class="w-full h-full px-7">
    <SearchBar class="my-5" />
    <div class="w-4/5 bg-white flex mx-auto" style="height: 79vh;">
      <div class="h-full w-2/12 overflow-y-auto px-5 py-3 border-r-2 ">
        <div class="font-bold text-sky-500 text-xl my-3">中图法分类</div>
        <div class="w-11/12 truncate my-1 hover:text-sky-500 hover:bg-slate-100 cursor-pointer"
          v-for="(item, index) in categories.data" :key="index">- {{
            item.CategoryName }}</div>
      </div>
      <div class="flex-1 overflow-y-auto p-5">
        <div>检索结果共有：{{ books.total }}条</div>
        <div>
          <div v-for="(item, index) in books.data" :key="index">
            <div>{{ item.BookID }} {{ item.Title }}</div>
            <div>
              <div><img :src="item.ImgUrl" alt=""></div>
              <div>
                <div>
                  <div>著者：{{ item.Author }}</div>
                  <div>出版信息：{{ item.PulishingHouse }},{{ formattedDate(item.PublicationDate) }}</div>
                </div>
                <div>
                  ISBN: {{ item.ISBN }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>