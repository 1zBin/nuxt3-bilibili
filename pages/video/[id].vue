<script setup lang="ts">
import type { BarrageInstance } from "vant";

// 弹幕相关
const barrageList = ref([
  { id: 100, text: "轻量" },
  { id: 101, text: "可定制的" },
  { id: 102, text: "移动端" },
  { id: 103, text: "Vue" },
  { id: 104, text: "组件库" },
  { id: 105, text: "VantUI" },
  { id: 106, text: "666" },
]);

const barrageRef = ref<BarrageInstance>();

const onPlay = () => {
  barrageRef.value?.play();
};

const onPause = () => {
  barrageRef.value?.pause();
};

// 通过路由参数获取视频id
const { params } = useRoute();
const { data: detail } = await useFetch(`/api/video/${params.id}`);

// 获取视频列表数据
const { data: videoList } = await useFetch("/api/video");

// 动态设置标题
useSeoMeta({
  title: `${detail.value?.title}_哔哩哔哩bilibili_${detail.value?.author.name}`,
});
</script>

<template>
  <!-- Sticky 粘性布局 -->
  <van-sticky>
    <!-- 头部 -->
    <AppHeader />
    <!-- 弹幕 -->
    <van-barrage v-model="barrageList" :auto-play="false" ref="barrageRef">
      <!-- 视频 -->
      <video
        controls
        class="video-play"
        ref="videoRef"
        @play="onPlay"
        @pause="onPause"
        :poster="detail?.pic"
        src="https://v95-web-sz.douyinvod.com/b22bc881df79722f6dc660461308cd33/6641a316/video/tos/cn/tos-cn-ve-15/ogBLAQAls8GLjATQIlDnAQbZErUUgXfeADVnc9/?a=6383&ch=11&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C3&cv=1&br=637&bt=637&cs=2&ds=4&ft=GZnU0RqeffPdXP~ka1jNvAq-antLjrKAtup.RkatoWBZvjVhWL6&mime_type=video_mp4&qs=15&rc=OmlnZzRnNzM3Nzk7ZzwzZUBpajN3aTM6ZnNtcDMzNGkzM0BiMmFfNl9jNmIxMmMtYjM2YSNgbG8xcjRvX15gLS1kLTBzcw%3D%3D&btag=80000e00028000&cquery=101n_100B_100H_100K_100o&dy_q=1715573872&feature_id=e585bce62f14c124a0ac1450c3a95af2&l=202405131217526CB4FAAD973773056406"
      ></video>
    </van-barrage>
  </van-sticky>
  <!-- 标题作者信息 -->
  <div class="info">
    <h1 class="title-text">{{ detail?.title }}</h1>
    <div class="body">
      <div class="author">
        <img class="avatar" :src="detail?.author.face" />
        <span class="name">{{ detail?.author.name }}</span>
      </div>
    </div>
  </div>
  <!-- 相关推荐 -->
  <div class="relate">
    <h3 class="relate-title">相关推荐</h3>
    <div class="relate-list">
      <AppVideo v-for="item in videoList" :key="item.aid" :item="item" />
    </div>
  </div>
</template>

<style scoped lang="scss">
.video-play {
  width: 100vw;
  height: auto;
  object-fit: contain;
  background-color: #fff;
}

.info {
  padding: 10px;
  border-bottom: 1px solid #eee;
  .title-text {
    font-size: 16px;
    font-weight: normal;
  }

  .body {
    display: flex;
    margin-top: 20px;
    justify-content: space-between;
    align-items: center;
  }

  .author {
    display: flex;
    align-items: center;
    .avatar {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      border: 1px solid #ccc;
    }
    .name {
      margin-left: 10px;
      font-size: 14px;
    }
  }
}

.relate {
  .relate-title {
    height: 32px;
    display: flex;
    align-items: center;
    font-size: 14px;
    font-weight: normal;
    padding: 0 10px;
    color: #333;
  }
  .relate-list {
    display: flex;
    flex-wrap: wrap;
    padding: 0 5px;
  }
}
</style>
