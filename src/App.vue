<script setup lang="ts">
import image from '@/assets/images/preview1.jpg'
import PictureMagnifyingGlass from '@/components/picture-magnifying-glass'
import VueUsePicMagnifyingGlass from '@/components/vueuse-pic-magnifying-glass'
import CanvasPicMagnifyingGlass from '@/components/canvas-pic-magnifying-glass'
import {reactive} from "vue";


const config1 = reactive({
  width: 240,
  height: 240,
  scale: 2
})

const config2 = reactive({
  width: 420,
  height: 240,
  scale: 3
})

const config3 = reactive({
  width: 400,
  height: 400,
  scale: 2
})
</script>

<template>
  <div>
<!--    <div>-->
<!--      <h3>基于事件监听实现：</h3>-->
<!--      <div>放大{{ config1.scale }}倍</div>-->
<!--      <div>宽{{ config1.width }}</div>-->
<!--      <div>高{{ config1.height }}</div>-->
<!--      <PictureMagnifyingGlass :picture="image" :config="config1"></PictureMagnifyingGlass>-->
<!--    </div>-->
<!--    <div>-->
<!--      <h3>基于事件监听实现：</h3>-->
<!--      <div>放大{{ config2.scale }}倍</div>-->
<!--      <div>宽{{ config2.width }}</div>-->
<!--      <div>高{{ config2.height }}</div>-->
<!--      <PictureMagnifyingGlass :picture="image" :config="config2"></PictureMagnifyingGlass>-->
<!--    </div>-->
<!--    <div>-->
<!--      <h3>基于@vueuse/core库实现：</h3>-->
<!--      <div>放大{{ config3.scale }}倍</div>-->
<!--      <div>宽{{ config3.width }}</div>-->
<!--      <div>高{{ config3.height }}</div>-->
<!--      <VueUsePicMagnifyingGlass :config="config3"></VueUsePicMagnifyingGlass>-->
<!--    </div>-->
    <CanvasPicMagnifyingGlass></CanvasPicMagnifyingGlass>
  </div>
</template>

<style scoped>
</style>
