<script setup>
const state = reactive({
  search: '',
  total: 0,
  page: 1,
  books: null,
  AllCategories: null,
  categories: null,
  categoryId: 0
})

// const categories = ref(null)
const route = useRoute()
state.search = route.query.s || ''

const getCategories = async () => {
  const { data: { value: result } } = await useFetch('http://localhost:7001/api/categories/all')
  state.AllCategories = result.data
}

const searchBooks = async () => {
  const { data: { value: result } } = await useFetch(`http://localhost:7001/api/books/home/search?search=${state.search}&page=${state.page}&categoryId=${state.categoryId}`)
  state.books = result.data
  state.total = result.total
  state.page = result.page
  const categories = []
  state.books.map(item => {
    if (categories.findIndex(itm => itm.CategoryID == item.Categories.CategoryID) !== -1) return
    categories.push({
      CategoryID: item.Categories.CategoryID,
      CategoryName: state.AllCategories.find(itm => itm.CategoryID == item.Categories.CategoryID).CategoryName
    })
  })
  categories.sort((a, b) => a.CategoryID - b.CategoryID)
  state.categories = categories
}

const handleSearch = async (s) => {
  state.page = 1
  state.search = s
  await searchBooks()
}

const defaultImg = (event) => {
  const img = event.srcElement
  img.src = '/book-default.jpg'
  // img.onerror = null
}

watch(() => state.page, (newValue, oldValue) => {
  searchBooks()
})



getCategories()
searchBooks()

</script>

<template>
  <div class="w-full h-full px-7">
    <SearchBar :s="state.search" @search="handleSearch" class="my-5" />
    <div class="w-4/5 bg-white flex mx-auto shadow-md" style="height: 79vh;">
      <div class="h-full w-2/12 overflow-y-auto px-5 py-3 border-r-2 ">
        <div class="font-bold text-sky-500 text-xl my-3">中图法分类</div>
        <div class="w-11/12 truncate my-1 hover:text-sky-500 hover:bg-slate-100 cursor-pointer"
          v-for="(item, index) in state.categories" :key="index">- {{
            item.CategoryName }}</div>
      </div>
      <div class="flex-1 overflow-y-auto p-5 divide-y divide-dashed innerbox">
        <div class="mb-7">检索结果共有：<span class="text-sky-500 font-bold">{{ state.total }}</span> 条</div>
        <div class="flex h-36 " v-for="(item, index) in state.books" :key="index">
          <div class="w-2/12 h-full flex items-center justify-center">
            <img :src="item.ImgUrl || '/book-default.jpg'" :onerror="defaultImg" class="h-4/5 mx-auto" />
          </div>
          <div class="flex-1 h-full flex  items-center">
            <el-descriptions class="margin-top w-full" :column="3" size="large" border>
              <el-descriptions-item>
                <template #label>
                  <div class="cell-item flex">
                    <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
                      <path fill="currentColor"
                        d="m512 863.36 384-54.848v-638.72L525.568 222.72a96 96 0 0 1-27.136 0L128 169.792v638.72l384 54.848zM137.024 106.432l370.432 52.928a32 32 0 0 0 9.088 0l370.432-52.928A64 64 0 0 1 960 169.792v638.72a64 64 0 0 1-54.976 63.36l-388.48 55.488a32 32 0 0 1-9.088 0l-388.48-55.488A64 64 0 0 1 64 808.512v-638.72a64 64 0 0 1 73.024-63.36z">
                      </path>
                      <path fill="currentColor" d="M480 192h64v704h-64z"></path>
                    </svg>
                    图书
                  </div>
                </template>
                {{ item.Title }}
              </el-descriptions-item>
              <el-descriptions-item>
                <template #label>
                  <div class="cell-item flex">
                    <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
                      <path fill="currentColor"
                        d="M512 512a192 192 0 1 0 0-384 192 192 0 0 0 0 384zm0 64a256 256 0 1 1 0-512 256 256 0 0 1 0 512zm320 320v-96a96 96 0 0 0-96-96H288a96 96 0 0 0-96 96v96a32 32 0 1 1-64 0v-96a160 160 0 0 1 160-160h448a160 160 0 0 1 160 160v96a32 32 0 1 1-64 0z">
                      </path>
                    </svg>
                    著者
                  </div>
                </template>
                {{ item.Author }}
              </el-descriptions-item>
              <el-descriptions-item>
                <template #label>
                  <div class="cell-item flex">
                    <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
                      <path fill="currentColor"
                        d="M832 384H576V128H192v768h640V384zm-26.496-64L640 154.496V320h165.504zM160 64h480l256 256v608a32 32 0 0 1-32 32H160a32 32 0 0 1-32-32V96a32 32 0 0 1 32-32zm160 448h384v64H320v-64zm0-192h160v64H320v-64zm0 384h384v64H320v-64z">
                      </path>
                    </svg>
                    ISBN
                  </div>
                </template>
                {{ item.ISBN }}
              </el-descriptions-item>
              <el-descriptions-item>
                <template #label>
                  <div class="cell-item flex">
                    <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
                      <path fill="currentColor"
                        d="M128 320v576h576V320H128zm-32-64h640a32 32 0 0 1 32 32v640a32 32 0 0 1-32 32H96a32 32 0 0 1-32-32V288a32 32 0 0 1 32-32zM960 96v704a32 32 0 0 1-32 32h-96v-64h64V128H384v64h-64V96a32 32 0 0 1 32-32h576a32 32 0 0 1 32 32zM256 672h320v64H256v-64zm0-192h320v64H256v-64z">
                      </path>
                    </svg>
                    出版信息
                  </div>
                </template>
                {{ item.PublishingHouse }}, {{ formattedDate(item.PublicationDate) }}
              </el-descriptions-item>
              <el-descriptions-item>
                <template #label>
                  <div class="cell-item flex">
                    <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
                      <path fill="currentColor"
                        d="M800 416a288 288 0 1 0-576 0c0 118.144 94.528 272.128 288 456.576C705.472 688.128 800 534.144 800 416zM512 960C277.312 746.688 160 565.312 160 416a352 352 0 0 1 704 0c0 149.312-117.312 330.688-352 544z">
                      </path>
                      <path fill="currentColor"
                        d="M512 512a96 96 0 1 0 0-192 96 96 0 0 0 0 192zm0 64a160 160 0 1 1 0-320 160 160 0 0 1 0 320z">
                      </path>
                    </svg>
                    位置
                  </div>
                </template>
                {{ item.Location }}
              </el-descriptions-item>
            </el-descriptions>
          </div>
        </div>
        <div class=" w-full flex items-center justify-center pt-7">
          <el-pagination class="text-center" v-model:current-page="state.page" background layout="prev, pager, next"
            :total="state.total" :page-size="13" :hide-on-single-page="true" />
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
svg {
  width: 18px;
  margin-right: 5px;
}

/*滚动条样式*/
.innerbox::-webkit-scrollbar {
  /*滚动条整体样式*/
  width: 5px;
  /*高宽分别对应横竖滚动条的尺寸*/
  height: 5px;
}

.innerbox::-webkit-scrollbar-thumb {
  /*滚动条里面小方块*/
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  background: rgba(14, 165, 233, 0.8);
}

.innerbox::-webkit-scrollbar-track {
  /*滚动条里面轨道*/
  -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  border-radius: 0;
  background: rgba(14, 165, 233, 0.1);
}
</style>