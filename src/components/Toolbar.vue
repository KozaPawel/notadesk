<script setup>
import { ref } from 'vue'

const props = defineProps({
  title: String
})

const x = ref(screen.width / 3)
const y = ref(screen.height / 4)
const mouseXPos = ref(0)
const mouseYPos = ref(0)
const isDragging = ref(false)

const dragging = () => {
  isDragging.value = true
}

window.addEventListener('mousemove', (e) => {
  if (isDragging.value) {
    const diffX = e.clientX - mouseXPos.value
    const diffY = e.clientY - mouseYPos.value

    x.value += diffX
    y.value += diffY
  }
  mouseXPos.value = e.clientX
  mouseYPos.value = e.clientY
})

window.addEventListener('mouseup', () => {
  isDragging.value = false

  const draggedItemX = x
  const draggedItemY = y

  if (draggedItemX.value < 0 || draggedItemX.value > window.innerWidth - 10) {
    x.value = 100
  }
  if (draggedItemY.value < 0 || draggedItemY.value > window.innerHeight - 60) {
    y.value = 100
  }
})

// window.addEventListener('touchmove', (e) => {
//   if (isDragging.value) {
//     const diffX = e.touches[0].clientX - mouseXPos.value
//     const diffY = e.touches[0].clientY - mouseYPos.value

//     x.value += diffX
//     y.value += diffY
//   }
//   mouseXPos.value = e.touches[0].clientX
//   mouseYPos.value = e.touches[0].clientY
// })
</script>

<template>
  <div
    class="window w-fit absolute transition delay-200 duration-300 ease-in-out"
    :style="{
      left: `${x}px`,
      top: `${y}px`
    }"
  >
    <div class="title-bar" @mousedown="dragging" @touchstart="dragging">
      <div class="title-bar-text select-none">{{ props.title }}</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" class="bg-[#dfdfdf]" @click="$emit('minimize')"></button>
        <button aria-label="Maximize" class="bg-[#dfdfdf]"></button>
        <button aria-label="Close" class="bg-[#dfdfdf]" @click="$emit('remove-me')"></button>
      </div>
    </div>
    <div class="window-body !m-0">
      <slot></slot>
    </div>
  </div>
</template>
