<script setup>
const props = defineProps({
  name: String,
  filename: String
})

function getImageUrl() {
  return new URL(`../assets/images/${props.filename}`, import.meta.url)
}
</script>
<template>
  <div
    class="flex flex-col items-center gap-1 w-20 cursor-pointer select-none border border-transparent border-dotted hover:border-[#000]"
    @click="$emit('add')"
  >
    <img class="w-8 h-8" :src="getImageUrl()" />
    <text class="text-white bg-[#00539f] w-full text-center">{{ props.name }}</text>
  </div>
</template>
