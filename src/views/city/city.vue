<template>
  <div class="city top-page">
    <div class="top">
      <!-- 1.搜索框 -->
      <van-search 
        v-model="searchValue" 
        placeholder="城市/区域/位置"
        shape="round"
        show-action
        @cancel="cancelClick"
      />

      <!-- 2.tab的切换 -->
      <van-tabs v-model:active="tabActive" color="#ff9854" line-height="3">
        <template v-for="(value, key, index) in allCities" :key="key">
          <van-tab :title="value.title" :name="key"></van-tab>
        </template>
      </van-tabs>
    </div>
    <div class="content">
      <template v-for="(value, key, index) in allCities">
        <!-- v-show 优化 -->
        <city-group v-show="tabActive === key" :group-data="value" />
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { storeToRefs } from 'pinia';
import useCityStore from '@/stores/modules/city';
import { useRouter } from 'vue-router';

import CityGroup from './components/city-group.vue'

const router = useRouter()

// 搜索框功能
const searchValue = ref("")
const cancelClick = () => {
  router.back()
}

// tab的切换
const tabActive = ref()

// 从Store中获取数据
const cityStore = useCityStore()
cityStore.fetchAllCitiesData()
const { allCities } = storeToRefs(cityStore)

</script>

<style lang="less" scoped>
.city {
  .top {
    position: relative;
    z-index: 9;
  }
  .content {
    height: calc(100vh - 98px);
    overflow-y: auto;
  }
}
</style>
