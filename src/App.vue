<template>
  <div id="app" class="w-3/5 mx-auto p-4 bg-gray-800 rounded-md m-4 relative overflow-y-hidden pt-14 mt-20">
    <div class="w-full absolute bg-purple-600 top-0 right-0 left-0 text-center text-white font-semibold text-xl py-2">
      <h1>Manpage of Todo</h1>
    </div>
    <form @submit.prevent="addNewTodo()" class="border-b-2 border-purple-600 py-4">
      <div class="flex items-center">
        <label class="text-white text-lg font-semibold">Todo Name: </label>
        <input v-model="newTodo" type="text" class="flex-1 px-2 py-1 bg-gray-300 rounded-sm text-gray-800">
      </div>
      <div class="mt-5 flex items-center justify-between">
        <button
          class="px-4 py-2 text-sm font-semibold text-white bg-purple-600 rounded-sm focus:outline-none hover:bg-purple-500 focus:ring-4 ring-purple-600 ring-opacity-25 transition duration-100 ease-in-out transform hover:scale-105"
        >
          Add Todo
        </button>
        <button
          class="p-3 rounded-full focus:outline-none hover:bg-purple-600 hover:bg-opacity-25"
          @click="markAllCompleted()"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 fill-current text-purple-600" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
          </svg>
        </button>
      </div>
    </form>

    <div id="todos" class="space-y-4 my-4">
      <div
        id="todo"
        v-for="todo in todos"
        :key="todo.id"
        @click="todoCompleted(todo)"
        class="py-4 px-2 rounded-lg bg-gray-300 cursor-pointer transform hover:scale-105 transition duration-100 ease-in-out flex items-center justify-between"
        :class="{'bg-purple-600 text-white line-through': todo.isCompleted}"
      >
        <h1>{{todo.text}}</h1>
        <span
          class="p-2 rounded-full cursor-pointer hover:bg-purple-700 hover:bg-opacity-20"
          :class="{'hover:bg-white text-white hover:bg-opacity-25': !todo.isCompleted}"
          @click="deleteTodo(todo)"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 fill-current" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
          </svg>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import {ref, onMounted } from 'vue'

export default {
  name: 'App',
  setup() {
    const newTodo = ref('')
    const todos = ref([])

    onMounted(()=> {
      if (localStorage.getItem('manTodos')) {
        let manTodos = JSON.parse(localStorage.getItem('manTodos'))
        manTodos.forEach(todo => {
          todos.value.push(todo)
        });
      }
    })
    function addNewTodo() {
      // console.log(newTodo.value);
      if (newTodo.value.trim()) {
        todos.value.push(
          {
            id: Date.now(),
            text: newTodo.value,
            isCompleted: false
          }
        )
        newTodo.value = ""
        localStorage.setItem('manTodos', JSON.stringify(todos.value))
      }
    }

    function todoCompleted(todo) {
      todo.isCompleted = !todo.isCompleted
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }

    function deleteTodo(todo) {
      todos.value = todos.value.filter( (item) => {
        return item != todo
      })
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }

    function markAllCompleted() {
      todos.value.forEach( (todo) => {
        todo.isCompleted = true
      })
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }

    return {
      newTodo,
      todos,
      addNewTodo,
      todoCompleted,
      deleteTodo,
      markAllCompleted
    }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
* {
  font-family: 'Anonymous Pro';
}
</style>