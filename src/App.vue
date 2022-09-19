<!-- 
  // Добавить emty list
  //  Добавить progress-bar
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
            Add product {{localeTime}} {{localeDate}}
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
          <button @click="markAllCompleted()"
            class="ml-4 bg-blue-300 hover:bg-blue-400 transition-all px-2 py-1 rounded text-white border border-blue-300">
            DO ALL
          </button>
        </form>
      </div>

      <div class="border rounded my-4">
      </div>

      <div>
        <div v-for="todo in todos" :key="todo.id" @click="todoCompleted(todo)"
          class="bg-gray-200 px-4 py-1 text-[18px] rounded border border-gray-300 flex justify-between items-center my-4">
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