<script setup>
import { ref } from 'vue'
import Toolbar from './Toolbar.vue'

const textareaContent = ref('')

const save = (filename) => {
  if (filename) {
    filename = prompt('Save file as...')
    if (filename == null) {
      return
    }
  }

  const textBlob = new Blob([textareaContent.value], { type: 'text/plain' })
  const fileUrl = URL.createObjectURL(textBlob)
  const downloadLink = document.createElement('a')

  downloadLink.href = fileUrl
  downloadLink.download = filename === '' ? 'Untitled' : filename
  document.body.appendChild(downloadLink)
  downloadLink.click()
  document.body.removeChild(downloadLink)
}
</script>

<template>
  <Toolbar :title="'Notepad'">
    <div class="flex gap-1 shadow-inner pl-1 w-full py-0.5 select-none">
      <text @click="save('')" class="px-1 hover:bg-blue-400 hover:text-white hover:cursor-pointer">
        Save
      </text>
      <text
        @click="save('filename')"
        class="px-1 hover:bg-blue-400 hover:text-white hover:cursor-pointer"
      >
        Save as...
      </text>
    </div>
    <textarea
      rows="8"
      class="p-2 -mb-1 overflow-scroll resize min-w-96 min-h-24 w-full h-full"
      v-model="textareaContent"
    ></textarea>
  </Toolbar>
</template>
