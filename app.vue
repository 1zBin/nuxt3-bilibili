<script setup lang="ts">
useSeoMeta({
  title: "哔哩哔哩 (゜-゜)つロ 干杯~-bilibili",
  description:
    "哔哩哔哩（bilibili.com)是国内知名的视频弹幕网站，这里有及时的动漫新番，活跃的ACG氛围，有创意的Up主。大家可以在这里找到许多欢乐。",
  keywords: "B站,bilibili,哔哩哔哩,哔哩哔哩动画,动漫,电影,在线动漫,高清电影",
  // 绕开 B 站防盗链
  referrer: "no-referrer",
});
</script>

<template>
  <!-- 路由链接 -->
  <!-- <NuxtLink to="/">首页</NuxtLink>
  <NuxtLink to="/video">视频页</NuxtLink> -->
  <!-- Nuxt 页面路由，相当于 RouterView -->
  <!-- 缓存 10 级页面，提升用户体验 -->
  <NuxtPage :keepalive="{ max: 10 }" />
</template>

<style lang="scss">
/* vant-ui 主题定制 */
:root {
  --van-primary-color: #fb7299 !important;
  --van-back-top-background: #fbfbfb !important;
  --van-back-top-text-color: #666 !important;
}
// 基础样式
@import "./assets/styles/base.scss";
// 字体图标
@import "./assets/styles/iconfont.scss";
</style>
