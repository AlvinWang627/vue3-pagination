<template>
  <div>pagination</div>
  <Pagination
    v-model:currentPage="currentPage"
    v-model:totalPage="totalPage"
    :click-handler="pageHandler"
    :prevText="'前一頁'"
    :nextText="'下一頁'"
  />
</template>

<script setup>
import { ref } from 'vue'
import Pagination from './components/Pagination.vue'
const currentPage = ref(1)
const totalPage = ref(20)
</script>

<style scoped></style>
