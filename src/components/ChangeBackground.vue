<script setup>
import { ref, onMounted } from 'vue'
import Toolbar from './Toolbar.vue'

const color = ref('#000000')

const changeBackground = () => {
  localStorage.setItem('background', JSON.stringify(color.value))
}

onMounted(() => {
  color.value = JSON.parse(localStorage.getItem('background'))
})
</script>

<template>
  <Toolbar :title="'Background'">
    <div class="resize overflow-hidden flex flex-col min-w-48 min-h-28">
      <text class="font-bold">Choose color</text>
      <input
        class="border-0 bg-transparent h-12 w-full hover:cursor-pointer"
        type="color"
        v-model="color"
      />
      <button @click="changeBackground(), $emit('change-background')">Change background</button>
    </div>
  </Toolbar>
</template>
