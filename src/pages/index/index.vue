<script setup lang="ts">
import { ref } from 'vue'
import { getHomeBannerAPI, getHomeCategoryAPI } from '@/services/home'
import CustomNavbar from './componets/CustomNavbar.vue'
import { onLoad } from '@dcloudio/uni-app'
import type { BannerItem, CategoryItem } from '@/types/home'
import CategoryPanel from './componets/CategoryPanel.vue'

// 获取首页轮播图数据
const bannerList = ref<BannerItem[]>([])
const getHomeBannerData = async () => {
  const res = await getHomeBannerAPI()
  // console.log('请求成功', res)
  bannerList.value = res.result
}

// 获取前台分类数据
const categoryList = ref<CategoryItem[]>([])
const getCategoryData = async () => {
  const res = await getHomeCategoryAPI()
  // console.log('请求成功', res)
  categoryList.value = res.result
}

onLoad(() => {
  getHomeBannerData()
  getCategoryData()
})
</script>

<template>
  <!-- 自定义导航栏 -->
  <CustomNavbar></CustomNavbar>
  <!-- 自定义轮播图 -->
  <XtxSwiper :list="bannerList"></XtxSwiper>
  <!-- 前台分类 -->
  <CategoryPanel :list="categoryList"></CategoryPanel>
</template>

<style lang="scss">
page {
  background-color: #f7f7f7;
}
</style>
