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
        <div>
          <h1 class="text-[24px] font-extrabold text-gray-500">Todo App</h1>
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
          <div @click="deleteTodo(todo)" class="bg-[#704bd6] text-[14px] border text-white cursor-pointer px-3 py-1 rounded-full">
            X
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