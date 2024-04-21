<script setup lang="ts">
import {computed, reactive, ref, watch} from "vue";
import {ConfigProps, MoveInfoProps} from "@/components/picture-magnifying-glass/interface"

const defConfig: ConfigProps = {
  maskColor: 'rgba(25, 122, 255, .5)',
  maskOpacity: .6,
  scale: 2,
  width: 420,
  height: 420
}

const props = defineProps({
  picture: Blob,
  config: Object
})
const config = reactive(defConfig)
watch(() => props.config, (value) => {
  if (!value) return
  Object.assign(config, value)
}, {
  immediate: true,
  deep: true
})
const maskTransform = ref<string>('transformMask:`translate(0px, 0px)`')
const showMask = ref(false) // 左侧遮罩层
const showMagnify = ref(false) // 右侧遮罩层
const moveInfo = reactive<MoveInfoProps>({
  left: 0,
  top: 0,
  pos: null
})
const maskRef = ref<HTMLDivElement | null>(null) // 遮罩层节点
const moveLeft = ref<string>('0')
const moveTop = ref<string>('0')

const getMaskWidth = computed(() => {
  return defConfig.width / defConfig.scale
})

const getMaskHeight = computed(() => {
  return defConfig.height / defConfig.scale
})

function handleMouseOver() {
  showMask.value = true
  showMagnify.value = true
}

function handleMouseMove(e: MouseEvent) {
  // 动态获取小图的位置（或者监听 scroll ）
  moveInfo.pos = maskRef.value!.getBoundingClientRect()
  if (!moveInfo.pos) return
  const { left, top, height, width } = moveInfo.pos!
  let x = e.clientX - left
  let y = e.clientY - top

  // 计算初始的遮罩左上角的坐标
  let maskX = x - getMaskWidth.value / 2
  let maskY = y - getMaskHeight.value / 2

  // 判断是否超出界限,并纠正
  maskY = maskY < 0 ? 0 : maskY
  maskX = maskX < 0 ? 0 : maskX

  if (maskY + getMaskHeight.value >= height) {
    maskY = height - getMaskHeight.value
  }
  if (maskX + getMaskWidth.value >= width) {
    maskX = width - getMaskWidth.value
  }

  // 遮罩移动
  maskTransform.value = `translate(${maskX}px, ${maskY}px)`

  // 背景图移动
  moveLeft.value = -maskX * defConfig.scale + 'px'
  moveTop.value = -maskY * defConfig.scale + 'px'
}

function handleMouseLeave() {
  showMask.value = false
  showMagnify.value = false
}

const bigWidth = computed(() => {
  return defConfig.scale * defConfig.width
})

const bigHeight = computed(() => {
  return defConfig.scale * defConfig.height
})

</script>

<template>
  <div class="pmg-wrap">
    <div
        class="left-picture-wrap"
        ref="maskRef"
        @mouseover="handleMouseOver"
        @mousemove="handleMouseMove"
        @mouseleave="handleMouseLeave"
        :style="{
          width: defConfig.width + 'px',
          height: defConfig.height + 'px'
        }">
      <img class="left-picture" :src="props.picture" alt="">
      <div class="magnify-wrap"
           :style="{
              background: defConfig.maskColor,
              height: getMaskHeight + 'px',
              width: getMaskWidth + 'px',
              opacity: defConfig.maskOpacity,
              transform: maskTransform
           }"
           v-show="showMask"
      ></div>
    </div>
    <div
        class="right-picture-wrap"
        v-show="showMagnify"
        :style="{
          width: defConfig.width + 'px',
          height: defConfig.height + 'px',
          left: defConfig.width + 20 + 'px'
        }">
      <div class="magnify-wrap"
           :style="{
             width: bigWidth + 'px',
             height: bigHeight + 'px',
             left: moveLeft,
             top: moveTop
           }">
        <div class="magnify"
             :style="{
               width: bigWidth - 2  + 'px',
               height: bigHeight - 2 + 'px'
             }">
          <img
              :src="picture"
              alt=""
              :style="{
                maxWidth: bigWidth - 2 + 'px',
                maxHeight: bigHeight -2 + 'px'
              }">
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.pmg-wrap {
  display: flex;
  margin: 24px;
  position: relative;
  .left-picture-wrap {
    position: relative;
    .left-picture {
      width: 100%;
      height: 100%;
      object-fit: contain;
      border: 1px solid #eee;
    }
    .magnify-wrap {
      position: absolute;
      top: 0;
      left: 0;
    }
  }
  .right-picture-wrap {
    overflow: hidden;
    position: absolute;
    border: 1px solid #eee;
    .magnify-wrap {
      position: absolute;
      top: 0;
      left: 0;
      .magnify {
        img {
          width: 100%;
          height: 100%;
          object-fit: contain;
        }
      }
    }
  }
}
</style>
