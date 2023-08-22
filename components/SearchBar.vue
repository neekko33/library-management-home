<script setup>
const search = ref('')
const props = defineProps({
  s: String
})
const emit = defineEmits(['search'])
const handleKeyDown = (key) => {
  if (key.key !== 'Enter') return
  emit('search', search.value)
}
const handleClick = () => {
  emit('search', search.value)
}

if (props.s) {
  search.value = props.s
}

</script>
<template>
  <div class="flex mx-auto w-2/5 justify-between">
    <div class="search bg-white flex items-center">
      <i class="searchIcon"></i>
      <div class="searchBar flex-1 pr-5">
        <input v-model="search" type="text" placeholder="请输入图书检索关键字" @keydown="handleKeyDown" />
      </div>
    </div>
    <div class="btn bg-sky-500 hover:bg-sky-400" @click="handleClick">
      馆内检索
    </div>
  </div>
</template>
<style scoped>
.search {
  width: 90%;
  vertical-align: middle;
  height: 56px;
  box-shadow: 0px 0px 3px 1px #dadada;
}

.btn {
  height: 56px;
  margin-left: 15px;
  width: 150px;
  text-align: center;
  line-height: 56px;
  color: #fff;
  border-radius: 2px;
  cursor: pointer;
}

.searchIcon {
  width: 17px;
  height: 16px;
  background: url(/ser-icon03.png) no-repeat;
  vertical-align: middle;
  display: inline-block;
  margin: 0 20px;
}

.searchBar input {
  font-size: 14px;
  width: 100%;
  height: 56px;
  line-height: 56px;
  color: #333;
  padding: 0 3px 0 10px;
  border: none;
  background: none;
  outline: none;
}
</style>