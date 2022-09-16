<!-- 
  // Добавить emty list
  //  
-->

<template>
  <main class="mt-4">
    <div class="max-w-[500px] border border-gray-200 bg-gray-100 m-auto py-4 px-8 rounded-xl">
      <div>
        <div>
          <h1 class="text-[24px] font-extrabold text-gray-500">MARKET LIST</h1>
        </div>

        <form @submit.prevent="addNewTodo()">
          <h1 class="text-[18px] mt-4 text-gray-500">
            Add product
          </h1>
          <input v-model="newTodo" class="text-[16px] px-2 py-1 rounded border outline-none" type="text">
          <button
            class="ml-4 bg-gray-300 hover:bg-gray-400 transition-all px-2 py-1 rounded text-white border border-gray-300">
            ADD
          </button>
          <button
            class="ml-4 bg-red-300 hover:bg-red-400 transition-all px-2 py-1 rounded text-white border border-red-300">
            CLEAR
          </button>
          <button
            @click="markAllCompleted()"
            class="ml-4 bg-blue-300 hover:bg-blue-400 transition-all px-2 py-1 rounded text-white border border-blue-300">
            DO ALL
          </button>
        </form>
      </div>

      <div class="border rounded my-4">
      </div>

      <div>
        <div v-for="todo in todos" :key="todo.id" @click="todoCompleted(todo)" class="bg-gray-200 px-4 py-1 text-[18px] rounded border border-gray-300 flex justify-between items-center my-4">
          <p :class="{'line-through': todo.isCompleted}">
            {{todo.text}}
          </p>
          <div @click="deleteTodo(todo)" class="bg-gray-300 border border-gray-400 cursor-pointer px-2 py-1 rounded">
            X
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  name: 'App',
  setup() {
    const newTodo = ref('')
    const todos = ref([])

    onMounted(() => {
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
      todos.value = todos.value.filter((item) => {
        return item != todo
      })
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }

    function markAllCompleted() {
      todos.value.forEach((todo) => {
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
* {
  font-family: 'Karla', sans-serif;
}
</style>