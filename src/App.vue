<!-- 
  // Добавить progress-bar
  // Добавить дату создания задачи
  // Добавить темную тему
  // Изменить UX
  // No Task | Emty
-->

<template>
  <main class="mt-4">
    <div class="max-w-[400px] min-h-[400px] border border-gray-200 bg-white m-auto py-4 px-8 rounded-xl">
      <div>
        <div class="flex justify-between items-center">
          <h1 class="text-[24px] font-extrabold text-gray-500">Todo App</h1>
          <div>
            <svg width="24" height="24" class="fill-gray-500" viewBox="0 0 16 16"><path fill="currentColor" class="fill-gray-500" d="M8 12a4 4 0 1 0 0-8a4 4 0 0 0 0 8zM8 0a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-1 0v-2A.5.5 0 0 1 8 0zm0 13a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-1 0v-2A.5.5 0 0 1 8 13zm8-5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1 0-1h2a.5.5 0 0 1 .5.5zM3 8a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1 0-1h2A.5.5 0 0 1 3 8zm10.657-5.657a.5.5 0 0 1 0 .707l-1.414 1.415a.5.5 0 1 1-.707-.708l1.414-1.414a.5.5 0 0 1 .707 0zm-9.193 9.193a.5.5 0 0 1 0 .707L3.05 13.657a.5.5 0 0 1-.707-.707l1.414-1.414a.5.5 0 0 1 .707 0zm9.193 2.121a.5.5 0 0 1-.707 0l-1.414-1.414a.5.5 0 0 1 .707-.707l1.414 1.414a.5.5 0 0 1 0 .707zM4.464 4.465a.5.5 0 0 1-.707 0L2.343 3.05a.5.5 0 1 1 .707-.707l1.414 1.414a.5.5 0 0 1 0 .708z"/></svg>
            <!-- <svg width="24" height="24" viewBox="0 0 16 16"><path fill="currentColor" class="fill-white" d="M6 .278a.768.768 0 0 1 .08.858a7.208 7.208 0 0 0-.878 3.46c0 4.021 3.278 7.277 7.318 7.277c.527 0 1.04-.055 1.533-.16a.787.787 0 0 1 .81.316a.733.733 0 0 1-.031.893A8.349 8.349 0 0 1 8.344 16C3.734 16 0 12.286 0 7.71C0 4.266 2.114 1.312 5.124.06A.752.752 0 0 1 6 .278z"/></svg> -->
          </div>
        </div>

        <form @submit.prevent="addNewTodo()">
          <h1 class="text-[18px] mt-4 text-gray-500">
            {{localeDate}} {{localeTime}}
          </h1>
          <div class="flex mt-2">
            <input v-model="newTodo" class="text-[16px] w-full px-2 py-1 rounded-l-lg border-[2px] outline-none" placeholder="New Task" type="text">
            <button
              class="bg-[#704bd6] hover:bg-[#5b3cb0] transition-all px-6 py-2 rounded-r-lg text-white">
              Add
            </button>
          </div>
          <!-- <button
            class="ml-4 bg-red-300 hover:bg-red-400 transition-all px-2 py-1 rounded text-white border border-red-300">
            CLEAR
          </button>
          <button @click="markAllCompleted()"
            class="ml-4 bg-blue-300 hover:bg-blue-400 transition-all px-2 py-1 rounded text-white border border-blue-300">
            DO ALL
          </button> -->
        </form>
      </div>

      <div>
        <div class="text-center font-medium text-[26px] text-gray-200 mt-[30%]" v-show="todos.length < 1">
          No Task
        </div>
        <div v-for="todo in todos" :key="todo.id" @click="todoCompleted(todo)"
          class="bg-gray-50 px-4 py-2 text-[18px] rounded shadow flex justify-between items-center my-4">
          <p :class="{'line-through': todo.isCompleted}">
            {{todo.text}}
          </p>
          <div @click="deleteTodo(todo)" class="bg-[#704bd6] text-[14px] border text-white cursor-pointer px-1 py-1 rounded-full">
            <svg width="14" height="14" viewBox="0 0 512 512"><path d="M437.5 386.6L306.9 256l130.6-130.6c14.1-14.1 14.1-36.8 0-50.9-14.1-14.1-36.8-14.1-50.9 0L256 205.1 125.4 74.5c-14.1-14.1-36.8-14.1-50.9 0-14.1 14.1-14.1 36.8 0 50.9L205.1 256 74.5 386.6c-14.1 14.1-14.1 36.8 0 50.9 14.1 14.1 36.8 14.1 50.9 0L256 306.9l130.6 130.6c14.1 14.1 36.8 14.1 50.9 0 14-14.1 14-36.9 0-50.9z" fill="currentColor"/></svg>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
// Импортируем ref и хук жизенного цикла onMounted из библиотеки vue
// Вызывается после того, как экземпляр компонента размонтирован. 
// Когда этот хук вызван, все директивы экземпляра компонента уже отвязаны, слушатели событий удалены, дочерние экземпляры размонтированы.
import { ref, onMounted } from 'vue'

export default {
  name: 'App',
  data: () => ({
    date: '',
    time: '',
  }),
  // Автообновление данных времени и даты
  created() {
    this.intervalId = setInterval(() => this.time = Date.now());
    this.intervalId = setInterval(() => this.date = Date.now());
  },
  // Получаем данные о дате и времени и обязятально делаем метод toLocaleDateString() (возвращает строку с языкозависимым представлением части с датой в этой дате.)
  computed: {
    localeDate() {
      return (new Date(this.date)).toLocaleDateString()
    },
    localeTime() {
      return (new Date(this.time)).toLocaleTimeString()
    },
  },
  setup() {
    const newTodo = ref('')
    const todos = ref([])

    // С помощью хука жизенного цикла записываем данные localStorage и пушим данные в список
    onMounted(() => {
      if (localStorage.getItem('manTodos')) {
        let manTodos = JSON.parse(localStorage.getItem('manTodos'))
        manTodos.forEach(todo => {
          todos.value.push(todo)
        });
      }
    })

    // Функция добавления новой задачи и записи в localStorage
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

    // Функция выполения задачи и записи в lS
    // Метод JSON.stringify()преобразует значение JavaScript в строку JSON, дополнительно заменяя значения, если указана функция-заменитель, или дополнительно включая только указанные свойства, если указан массив-заменитель.
    function todoCompleted(todo) {
      todo.isCompleted = !todo.isCompleted
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }

    // Функция удаления и запись в lS
    function deleteTodo(todo) {
      todos.value = todos.value.filter((item) => {
        return item != todo
      })
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }

    // Функция выполение всех задач
    function markAllCompleted() {
      todos.value.forEach((todo) => {
        todo.isCompleted = true
      })
      localStorage.setItem('manTodos', JSON.stringify(todos.value))
    }


    // Возвращаем из функции 
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