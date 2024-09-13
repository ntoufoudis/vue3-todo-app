<script setup lang="ts">
import { onMounted, type Ref, ref } from 'vue'

const newTodo = ref('')

interface todo {
  completed: boolean,
  text: string
}

const todos: Ref<todo[]> = ref([])

onMounted(() => {
  const parsedJSON = JSON.parse(localStorage.getItem('todos') || '""')
  for (const item of parsedJSON) {
    todos.value.push(item)
  }
})

function addTodo() {
  if (newTodo.value !== '') {
    todos.value.push({
      completed: false,
      text: newTodo.value
    })
    newTodo.value = ''
    updateStorage()
  }
}

function removeAllTodos() {
  todos.value.splice(0, todos.value.length)
  updateStorage()
}

function removeTodo(index: any) {
  todos.value.splice(index, 1)
  updateStorage()
}

function completedTodo(todo: any) {
  todo.completed = !todo.completed
  updateStorage()
}

function updateStorage() {
  localStorage.setItem('todos', JSON.stringify(todos.value))
}
</script>

<template>
  <div class="container mx-auto px-10 md:px-0">
    <div class="grid grid-cols-1 md:grid-cols-4">
      <div class="col-span-1 md:col-span-2 md:col-start-2">
        <div class="mt-10">
          <h1 class="text-5xl text-center font-semibold text-gray-900">
            ToDo App
          </h1>
          <div class="mt-6">
            <form @submit.prevent="addTodo()">
              <div class="grid grid-cols-1 gap-4">
                <div>
                  <input
                    class="border-2 outline-none py-2 px-2 shadow-md font-medium w-full rounded-md border-purple-500/50
                      hover:border-purple-500 focus:border-purple-500 focus:shadow-purple-500"
                    v-model="newTodo"
                    autofocus
                  />
                </div>
                <div class="grid grid-cols-2 gap-2">
                  <div>
                    <button
                      class="text-white py-2 px-4 shadow-md w-full rounded bg-blue-400 hover:bg-blue-600 font-semibold"
                    >
                      Add Todo
                    </button>
                  </div>
                  <div>
                    <button
                      class="text-white py-2 px-4 shadow-md w-full rounded bg-red-400 hover:bg-red-600 font-semibold"
                      v-if="todos.length !== 0"
                      @click="removeAllTodos"
                    >
                      Remove All Todos
                    </button>
                  </div>
                </div>
              </div>
            </form>
          </div>
          <div class="mt-8 text-center">
            <div id="todoList">
              <div class="grid grid-cols-1 gap-3">
                <div class="h-full" v-if="todos.length === 0">
                  <p class="text-gray-400">It appears you didn't add any ToDo.</p>
                </div>
                <div
                  class="rounded shadow-md p-3 h-full hover:shadow-gray-400 text-gray-600 text-lg font-semibold flex
                    justify-between items-center text-left"
                  :class="{ 'line-through': todo.completed }"
                  v-for="(todo, index) in todos"
                  :key="index"
                >
                  <span
                    class="text-gray-600 text-lg font-semibold w-full"
                    @click="completedTodo(todo)"
                  >
                    {{ todo.text }}
                  </span>
                  <button
                    class="text-red-300 py-2 px-4 shadow-md rounded-full bg-gray-100 hover:text-red-600 font-semibold"
                    @click="removeTodo"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="size-5"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005
                          13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0
                          1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0
                          0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39
                          3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355
                          5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1
                          0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
