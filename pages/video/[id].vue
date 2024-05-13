<script setup lang="ts">
import type { BarrageInstance } from "vant";

// 弹幕相关
const barrageList = ref([
  { id: 100, text: "iKun" },
  { id: 101, text: "爱坤" },
  { id: 102, text: "黑子在哪" },
  { id: 103, text: "冷知识:一坤年等于两年半" },
  { id: 104, text: "鸡你太美" },
  { id: 105, text: "纪录片" },
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
        :volume="0.5"
        src="https://v5-hl-tx-ov.douyinvod.com/c4debcdfdb696d3d5fb8f7057aae6ad2/6641ccc7/video/tos/cn/tos-cn-ve-15c001-alinc2/oAYxRUgyQDzsQRPAoDlAVneBqI9rC0nbgAHe99/?a=6383&ch=11&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C3&cv=1&br=426&bt=426&cs=2&ds=3&ft=CZdgCYlIDyjNNRVQ9waLc1Mhd.svk4XR3-ApQX&mime_type=video_mp4&qs=15&rc=aDM8NWc0OjY2NzhnPGk8ZEBpanNzd2Q6ZnY7cDMzNGkzM0BfXmMzXy8tNmExMDUyXzJeYSNwaGIucjRfNWlgLS1kLS9zcw%3D%3D&btag=80000e00028000&cquery=101n_100B_100x_100z_100o&dy_q=1715584545&feature_id=c6de0308cacfd993ef282c8e1c646267&l=2024051315154453EF2FA3FF6EB6013960"
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
