<script setup lang="ts">
import { ref } from 'vue'
import { getHomeBannerAPI, getHomeCategoryAPI, getHomeHotAPI } from '@/services/home'
import CustomNavbar from './componets/CustomNavbar.vue'
import { onLoad } from '@dcloudio/uni-app'
import type { BannerItem, CategoryItem, HotItem } from '@/types/home'
import CategoryPanel from './componets/CategoryPanel.vue'
import HotPanel from './componets/HotPanel.vue'
import type { XtxGuessInstance } from '@/types/component'

// 获取首页轮播图数据
const bannerList = ref<BannerItem[]>([])
const getHomeBannerData = async () => {
  const res = await getHomeBannerAPI()
  bannerList.value = res.result
}

// 获取前台分类数据
const categoryList = ref<CategoryItem[]>([])
const getHomeCategoryData = async () => {
  const res = await getHomeCategoryAPI()
  categoryList.value = res.result
}

// 获取热门推荐数据
const hotList = ref<HotItem[]>([])
const getHomeHotData = async () => {
  const res = await getHomeHotAPI()
  hotList.value = res.result
}

onLoad(() => {
  getHomeBannerData()
  getHomeCategoryData()
  getHomeHotData()
})

// 获取猜你喜欢组件实例
const guessRef = ref<XtxGuessInstance>()
// 触底事件
const onScrolltolower = () => {
  console.log('触底了')
  guessRef.value?.getMore()
}
</script>

<template>
  <!-- 自定义导航栏 -->
  <CustomNavbar></CustomNavbar>
  <!-- 滚动容器 -->
  <scroll-view @scrolltolower="onScrolltolower" class="scroll-view" scroll-y>
    <!-- 自定义轮播图 -->
    <XtxSwiper :list="bannerList"></XtxSwiper>
    <!-- 前台分类 -->
    <CategoryPanel :list="categoryList"></CategoryPanel>
    <!-- 热门推荐 -->
    <HotPanel :list="hotList"></HotPanel>
    <!-- 猜你喜欢 -->
    <XtxGuess ref="guessRef"></XtxGuess>
  </scroll-view>
</template>

<style lang="scss">
page {
  background-color: #f7f7f7;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.scroll-view {
  flex: 1;
}
</style>
