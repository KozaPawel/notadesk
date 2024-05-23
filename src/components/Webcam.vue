<script setup>
import { ref, onMounted } from 'vue'
import Toolbar from './Toolbar.vue'
import WebcamFilters from './WebcamFilters.vue'

const width = ref(0)
const height = ref(0)
const activeFilter = ref('')

const getCamera = async () => {
  try {
    await navigator.mediaDevices
      .getUserMedia({
        video: true
      })
      .then((stream) => {
        const video = document.querySelector('video')
        video.srcObject = stream
      })
  } catch (error) {
    console.log("Can't use camera.")
    console.log(error)
  }
}

const changeActiveFilter = (value) => {
  activeFilter.value = value[0]
}

onMounted(() => {
  getCamera()
  setTimeout(() => {
    width.value = document.getElementById('video').videoWidth
    height.value = document.getElementById('video').videoHeight
  }, 200)
})
</script>

<template>
  <Toolbar :title="'Webcam'">
    <div
      class="resize-x overflow-hidden min-w-96 min-h-72"
      :style="{
        maxWidth: `${width}px`,
        maxHeight: `${height}px`
      }"
    >
      <video
        id="video"
        autoplay
        class="bg-black"
        :class="{
          sepia: activeFilter === 'sepia',
          grayscale: activeFilter === 'grayscale',
          invert: activeFilter === 'invert',
          'hue-rotate-30': activeFilter === 'hue30',
          'hue-rotate-90': activeFilter === 'hue90',
          'saturate-200': activeFilter === 'saturation'
        }"
      ></video>
    </div>
    <div class="w-full px-0.5 pb-1">
      <text class="font-bold text-lg">Filter</text>
      <WebcamFilters :activeFilter="activeFilter" @change-filter="changeActiveFilter" />
    </div>
  </Toolbar>
</template>
