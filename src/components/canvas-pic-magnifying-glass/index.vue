<script lang="ts" setup>
import { MagnifyGlass } from '@/libs/magnify-glass'
import "@/libs/magnify-glass/index.css"
import {ref} from "vue";

const magnifyGlass = new MagnifyGlass()
const isActive = ref(false)
function showGlass() {
  isActive.value = !isActive.value
  if (isActive.value) {
    magnifyGlass.mount()
  } else {
    magnifyGlass.destroy()
  }
}

</script>

<template>
  <div class="cpmg-wrap">
    <div class="glass-wrap" @click="showGlass">放大镜</div>
  </div>
</template>

<style>
.cpmg-wrap {
  width: 100vw;
  height: 100vh;
  background-image: url("@/assets/images/preview1.jpg");
  background-size: cover;
  position: relative;
  .glass-wrap {
    position: absolute;
    right: 0;
    height: 80px;
    width: 120px;
    top: 50%;
    transform: translateY(-50%);
    display: flex;
    justify-content: center;
    align-items: center;
    background: #fff;
    cursor: pointer;
  }
}
</style>