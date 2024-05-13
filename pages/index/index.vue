<script setup lang="ts">
import type { VideoItem } from "@/types/video";
// get /api/channel
const { data: channelList } = await useFetch("/api/channel");
console.log("channel", channelList.value);
const { data: videoList } = await useFetch("/api/video");
console.log("videoList", videoList.value);

// 显示的列表
const list = ref<VideoItem[]>([]);
// 加载状态
const loading = ref(false);
// 是否加载完成
const finished = ref(false);
// 页码
let page = 1;
let pageSize = 20;
//滚动到底部触发
const onLoad = () => {
  loading.value = false;
  // 根据当前页码提取数据
  const data = videoList.value?.slice(
    (page - 1) * pageSize,
    page * pageSize
  ) as VideoItem[];
  // 追加到用于渲染的数组中
  list.value.push(...data);
  // 页码累加
  page++;
  // 加载完成
  if (videoList.value?.length === list.value.length) {
    finished.value = true;
  }
};

// 初始化加载-主动请求前20条数据，用户首屏渲染，方便SEO抓取到数据
onLoad();
</script>

<template>
  <AppHeader />
  <!-- 频道模块 -->
  <van-tabs>
    <van-tab v-for="item in channelList" :key="item.id" :title="item.name" />
  </van-tabs>
  <!-- 视频列表 -->

  <van-list
    v-model:loading="loading"
    :finished="finished"
    finished-text="没有更多了"
    @load="onLoad"
  >
    <div class="video-list">
      <AppVideo v-for="item in list" :key="item.bvid" :item="item" />
    </div>
  </van-list>
</template>

<style lang="scss">
// 视频列表
.video-list {
  display: flex;
  flex-wrap: wrap;
  padding: 10px 5px;
}
</style>
