<script lang="ts" setup>
import { useMouseInElement } from '@vueuse/core'
import image from '@/assets/images/preview1.jpg'
import {computed, reactive, ref, watch} from "vue";
import {ConfigProps} from "./interface"

const defConfig: ConfigProps = {
  maskColor: 'rgba(25, 122, 255, .5)',
  maskWidth: 200,
  maskHeight: 200,
  maskOpacity: .6,
  scale: 2,
  width: 400,
  height: 400
}

const props = defineProps({
  image: {
    type: Blob
  },
  config: Object
})
const config = reactive(defConfig)

watch(() => props.config, (value) => {
  if (!value) return
  Object.assign(config, value)
}, {
  deep: true,
  immediate: true
})

const target = ref(null)
const { isOutside, elementX, elementY } = useMouseInElement(target)

const position = computed(() => {
  let x = elementX.value - config.maskWidth / 2 // -100 让光标处再中间
  let y = elementY.value - config.maskWidth / 2
  // 边界处理
  x = x < 0 ? 0 : x
  y = y < 0 ? 0 : y
  x = x + config.maskWidth > config.width ? config.width - config.maskWidth : x
  y = y + config.maskHeight > config.height ? config.height - config.maskHeight : y
  return {
    x,
    y
  }
})

</script>

<template>
  <div id="mouse-move-info">
    {{isOutside}}
    X: {{elementX}}
    Y: {{elementY}}
  </div>
  <div class="vpmg-wrap">
    <div
        class="left-wrap"
        ref="target"
        :style="{
          width: config.width + 'px',
          height: config.height + 'px'
        }"
    >
      <img :src="image" alt=""  />
      <!-- 移动遮罩 -->
      <div
          class="layer"
          ref="target"
          v-show="!isOutside"
          :style="{
            left: position.x + 'px',
            top: position.y + 'px',
            background: config.maskColor,
            width: config.maskWidth + 'px',
            height: config.maskHeight + 'px'
          }"
      ></div>
    </div>
    <!-- 显示在右侧的放大之后的区域 -->
    <div class="right-wrap"
         v-show="!isOutside"
         :style="[{
           backgroundImage:'url(' + image + ')',
           backgroundSize: `${config.width * config.scale}px ${config.height * config.scale}px`,
           backgroundPosition: `-${position.x * config.scale}px -${position.y * config.scale}px`,
           left: config.width + 20 + 'px'
         }]"
    ></div>
  </div>
</template>

<style lang="less">
.vpmg-wrap {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  z-index: 500;
  margin: 24px;
  .right-wrap {
    position: absolute;
    top: 0;
    width: 400px;
    height: 400px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    background-repeat: no-repeat;
    background-color: #f8f8f8;
    background-size: cover;
  }
  .left-wrap {
    background: #f5f5f5;
    position: relative;
    cursor: move;
    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
    .layer {
      left: 0;
      top: 0;
      // 可以移动
      position: absolute;
    }
  }
}
</style>