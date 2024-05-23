<script setup>
import { ref, onMounted } from 'vue'
import Notepad from './components/Notepad.vue'
import Webcam from './components/Webcam.vue'
import Todo from './components/Todo.vue'
import DesktopIcon from './components/DesktopIcon.vue'
import ChangeBackground from './components/ChangeBackground.vue'

const topComponent = ref(0)
const time = ref(0)
const backgroundColor = ref('')
const apps = ref([])
const opened = ref({
  webcam: false,
  todo: false,
  background: false
})

const addComponent = (componentName) => {
  for (let x = 0; x < Object.entries(opened.value).length; x++) {
    if (
      componentName.toLowerCase() === Object.entries(opened.value)[x][0] &&
      Object.entries(opened.value)[x][1]
    ) {
      return
    }
  }

  cannotOpen(componentName)

  apps.value.push({
    type: componentName,
    key: Math.random(),
    visible: true
  })
}

const removeComponent = (componentIndex, componentName) => {
  canOpen(componentName)
  apps.value.splice(componentIndex, 1)
}

const canOpen = (componentName) => {
  for (let x = 0; x < Object.entries(opened.value).length; x++) {
    if (componentName.toLowerCase() === Object.entries(opened.value)[x][0]) {
      const matchedName = Object.entries(opened.value)[x][0]
      opened.value[matchedName] = false
    }
  }
}

const cannotOpen = (componentName) => {
  for (let x = 0; x < Object.entries(opened.value).length; x++) {
    if (componentName.toLowerCase() === Object.entries(opened.value)[x][0]) {
      const matchedName = Object.entries(opened.value)[x][0]
      opened.value[matchedName] = true
    }
  }
}

const changeTopComponent = (componentIndex) => {
  topComponent.value = componentIndex
}

const getTime = async () => {
  const today = new Date()
  time.value =
    (today.getHours() < 10 ? '0' + today.getHours() : today.getHours()) +
    ':' +
    (today.getMinutes() < 10 ? '0' + today.getMinutes() : today.getMinutes()) +
    ':' +
    (today.getSeconds() < 10 ? '0' + today.getSeconds() : today.getSeconds())
}

const changeBackground = () => {
  localStorage.getItem('background') ??
    localStorage.setItem('background', JSON.stringify('#008080'))

  backgroundColor.value = JSON.parse(localStorage.getItem('background'))
}

onMounted(() => {
  changeBackground()
  getTime()
  setInterval(() => getTime(), 1000)
})
</script>

<template>
  <div
    class="flex flex-col justify-between h-screen w-screen relative overflow-hidden"
    :style="{
      backgroundColor: `${backgroundColor}`
    }"
  >
    <!-- content -->
    <div class="flex flex-col pt-1 gap-2">
      <DesktopIcon :name="'Notepad'" :filename="'notepad.png'" @add="addComponent('Notepad')" />
      <DesktopIcon :name="'Todo'" :filename="'calendar.png'" @add="addComponent('Todo')" />
      <DesktopIcon :name="'Webcam'" :filename="'camera.png'" @add="addComponent('Webcam')" />
      <DesktopIcon
        :name="'Background'"
        :filename="'background.png'"
        @add="addComponent('Background')"
      />

      <div v-if="apps.length">
        <div v-for="(component, index) in apps" :key="component.key">
          <component
            :is="
              component.type === 'Notepad'
                ? Notepad
                : component.type === 'Webcam'
                  ? Webcam
                  : component.type === 'Todo'
                    ? Todo
                    : ChangeBackground
            "
            :class="{ 'z-50': topComponent === index, hidden: !component.visible }"
            @remove-me="removeComponent(index, component.type)"
            @minimize="changeTopComponent(-1), (component.visible = false)"
            @mousedown="changeTopComponent(index)"
            @change-background="changeBackground()"
          ></component>
        </div>
      </div>
    </div>

    <!--footer-->
    <div class="window">
      <div class="flex justify-between status-bar">
        <div class="flex gap-0.5">
          <button class="flex items-center justify-center gap-1 font-bold tracking-wide">
            <img class="w-6 h-6" src="./assets/images/windows.png" />
            <text>Start</text>
          </button>
          <div class="overflow-hidden w-fit h-6">
            <button
              v-for="(component, index) in apps"
              :key="component.key"
              :class="{ 'font-bold shadow-inner': index === topComponent }"
              class="h-full px-2 text-left"
              @click="changeTopComponent(index), (component.visible = true)"
            >
              {{ component.type }}
            </button>
          </div>
        </div>
        <p class="px-2 py-0.5 shadow-inner min-w-16 text-center content-center">{{ time }}</p>
      </div>
    </div>
  </div>
</template>
