<script setup>
import { ref, onMounted } from 'vue'
import Toolbar from './Toolbar.vue'

const newTodo = ref('')
const todos = ref([])

const addTodo = () => {
  if (newTodo.value !== '') {
    todos.value.push({ name: newTodo.value, status: 'doing' })
    newTodo.value = ''
    localStorage.setItem('todoList', JSON.stringify(todos))
  }
}

const todoDone = (index) => {
  todos.value[index].status = 'done'
  localStorage.setItem('todoList', JSON.stringify(todos))
}

const deleteTodo = (index) => {
  todos.value.splice(index, 1)
  localStorage.setItem('todoList', JSON.stringify(todos))
}

const loadTodos = () => {
  const storage = JSON.parse(localStorage.getItem('todoList'))
  todos.value = storage !== null ? storage._value : []
}

onMounted(() => {
  loadTodos()
})
</script>

<template>
  <Toolbar :title="'Todo'">
    <div class="resize overflow-hidden min-w-96 min-h-72 h-32 p-1">
      <div class="flex flex-col h-full gap-1 mb-2">
        <form @submit.prevent="addTodo" class="flex gap-1">
          <input class="w-full !h-6" type="text" v-model="newTodo" />
          <button type="submit" class="h-6">Add</button>
        </form>
        <div class="h-full pr-1 overflow-y-scroll mb-2">
          <div v-for="(todo, index) in todos" :key="index" class="w-full">
            <div v-if="todo.status !== 'done'">
              <div class="flex justify-between gap-1 w-full h-6">
                <text class="font-bold">{{ todo.name }}</text>
                <button class="h-6" @click="todoDone(index)">Done</button>
              </div>
              <hr class="border-[#8b8b8b] my-1" />
            </div>
          </div>
          <div v-for="(todo, index) in todos" :key="index" class="w-full">
            <div v-if="todo.status === 'done'">
              <div class="flex justify-between gap-1 w-full">
                <text class="font-bold opacity-70">{{ todo.name }}</text>
                <button class="h-6 text-red-600" @click="deleteTodo(index)">Delete</button>
              </div>
              <hr class="border-[#8b8b8b] my-1" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </Toolbar>
</template>
