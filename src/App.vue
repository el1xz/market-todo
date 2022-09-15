<!-- 
// Todo
// Приложение покупки
// Сделать UX
// Добавить категории магазинов
// Добавить фильтр по категориям
// Добавить кнопку удаления всех товаров 
// Добавить progressbar для задач 
-->


<script setup>
// Импортируем нужные функции из Vue
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

// Метод sort() на месте сортирует элементы массива и возвращает отсортированный массив.
const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  // Проверка на пустые поля (пустое занчение value)
  // Если правило соблюдается (пустое значение) ничего не возвращает, тем самым не добавляет задачу
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }
  // Добавляем значение из поля ввода
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })
  // Обнуляем поле для ввода задачи 
  input_content.value = ''
  input_category.value = null
}

// Удаление задачи
const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}

// Запись задач в localStorage
// Метод JSON.stringify() преобразует значение JavaScript в строку JSON, возможно с заменой значений, если указана функция замены, или с включением только определённых свойств, если указан массив замены.
watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
},
  { deep: true }
)

// Отслеживаем значение name с помощью функции watch и передаем данные в localStorage
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

// Получаем данные из localStorage с помощью хука onMounted
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  // Метод JSON.parse() разбирает строку JSON, возможно с преобразованием получаемого в процессе разбора значения.
  todos.value = JSON.parse(localStorage.getItem('todos')) || '[]'
})
</script>

<template>
  <main>
    <section>
      <h1>
        Привет, <input type="text" placeholder="ваше имя" v-model="name">
      </h1>
    </section>

    <section>
      <h1>
        Создать задачу
      </h1>

      <form @submit.prevent="addtodo">
        <h1>
          Что в вашем списке задач?
        </h1>
        <input type="text" placeholder="создать" v-model="input_content">

        <h1>
          Выбрать категорию
        </h1>

        <div>
          <input type="radio" name="category" value="бизнес" v-model="input_category">
          <div>бизнес</div>
        </div>
        <div>
          <input type="radio" name="category" value="персональная" v-model="input_category">
          <div>персональная</div>
        </div>

        <button @click="addTodo">
          Добавить
        </button>
      </form>
      <div>
        <h1>
          Список задач:
        </h1>
        <div v-for="todo in todos_asc">
          <input type="text" v-model="todo.content">
          <button @click="removeTodo(todo)">Удалить</button>
        </div>
      </div>
    </section>
  </main>
  <footer class="bg-gray-800 text-white footer py-4 px-4">
    <div class="flex justify-between">
      <a target=_blank class="text-gray-400 hover:text-white transition-all"
        href="https://www.youtube.com/watch?v=qhjxAP1hFuI">Video</a>
      <a target=_blank class="text-gray-400 hover:text-white transition-all"
        href="https://github.com/TylerPottsDev/yt-vue-todo-2022/blob/master/src/App.vue">Project GIT</a>
      <a target=_blank class="text-gray-400 hover:text-white transition-all" href="https://github.com/el1xz">el1xz ©</a>
    </div>
  </footer>
</template>

<style>
.content {
  width: fit-content;
}
</style>
