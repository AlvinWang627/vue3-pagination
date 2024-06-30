<template>
  <div class="pagination-container">
    <div @click="changePageHandler(1)" v-if="firstPageButtonShow">{{ firstPageText }}</div>
    <div @click="changePageHandler(currentPage - 1)">{{ prevText }}</div>
    <div
      @click="changePageHandler(page)"
      v-for="page in renderPager"
      :key="page"
      :style="{
        color: currentPage === page ? 'red' : 'blue'
      }"
    >
      {{ page }}
    </div>
    <div @click="changePageHandler(currentPage + 1)">{{ nextText }}</div>
    <div @click="changePageHandler(totalPage)" v-if="endPageButtonShow">{{ endPageText }}</div>
  </div>
</template>

<script setup>
// TODO:
// 1. object the page
// 2. insert the ellipsis in the page object
// 3. pageRange e.g. pageRange = 2, current page = 10, currentPage -+ 2 renderPage = [1, 2, ... ,  8, 9, 10, 11, 12, ... , 19, 20]

import { computed } from 'vue'

const currentPage = defineModel('defineModel', {
  type: Number,
  required: true,
  default: 1
})
const totalPage = defineModel('totalPage', {
  type: Number,
  required: true,
  default: 10
})

const props = defineProps({
  prevText: { type: String, default: 'Prev' },
  nextText: { type: String, default: 'Next' },
  firstPageText: {
    type: String,
    default: 'First'
  },
  endPageText: {
    type: String,
    default: 'End'
  },
  firstPageButtonShow: {
    type: Boolean,
    default: false
  },

  endPageButtonShow: {
    type: Boolean,
    default: false
  },

  containerClass: { type: String, default: 'className' }
})

const renderPager = computed(() => {
  const renderArr = Array.from({ length: totalPage.value }, (v, i) => i + 1)
  if (totalPage.value <= 5) return renderArr
  let filterArr = []

  if (currentPage.value <= 3) {
    filterArr = [...renderArr.slice(0, 4), '...', totalPage.value]
  } else if (currentPage.value >= totalPage.value - 2) {
    filterArr = [1, '...', ...renderArr.slice(totalPage.value - 4, totalPage.value)]
  } else {
    filterArr = [
      1,
      '...',
      ...renderArr.slice(currentPage.value - 2, currentPage.value + 1),
      '...',
      totalPage.value
    ]
  }

  return filterArr
})

const changePageHandler = (page) => {
  if (page === currentPage.value) return
  if (page !== Number(page)) return
  if (page < 1) {
    page = 1
  } else if (page > props.totalPage) {
    page = props.totalPage
  }
  currentPage.value = page
}
</script>

<style scoped lang="scss">
.pagination-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 200px;

  div {
    margin: 0 30px;
    cursor: pointer;
    padding: 5px 10px;
    border: 1px solid #ccc;
  }
}
</style>
