<script setup>
import { ref } from 'vue'

const props = defineProps({
  activeFilter: String
})

const filters = ref({
  sepia: false,
  grayscale: false,
  invert: false,
  hue30: false,
  hue90: false,
  saturation: false
})
</script>

<template>
  <div class="grid grid-cols-6 gap-1">
    <button
      v-for="filter in Object.entries(filters)"
      :key="filter"
      class="col-span-2 px-2 text-left text-black"
      :class="{ 'shadow-inner': props.activeFilter === filter[0] }"
      @click="
        props.activeFilter !== filter[0]
          ? $emit('change-filter', filter)
          : $emit('change-filter', 0)
      "
    >
      {{ filter[0].charAt(0).toUpperCase() + filter[0].slice(1) }}
    </button>
  </div>
</template>
