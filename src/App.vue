<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const inputContent = ref('')
const inputCategory = ref(null)

const todosAsc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {
  deep: true
})

const addTodo = () => {
  if (inputContent.value.trim() === '' || inputCategory.value === null) {
    return
  }

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })

  inputContent.value = ''
}

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <main class="app container mx-auto mt-2 p-2">

    <section class="greeting">
      <h2 class="title text-2xl font-semibold mb-5">
        What's up, <input class="bg-transparent outline-none" type="text" id="name" placeholder="Name here"
          v-model="name">
      </h2>
    </section>

    <section class="create-todo">
      <h3 class="mb-1">CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4 class="mb-5 text-gray-500">What's on your todo list?</h4>
        <input class="w-full p-4 text-lg rounded-2xl bg-white shadow mb-5 outline-none" type="text" name="content"
          id="content" placeholder="e.g. make a video" v-model="inputContent" />

        <h4 class="mb-5 text-gray-500">Pick a category</h4>
        <div class="options flex justify-between gap-4">

          <label class="bg-white flex justify-center items-center flex-col p-6 rounded-xl cursor-pointer shadow">
            <input type="radio" name="category" id="category1" value="business" v-model="inputCategory" />
            <span class="bubble business mb-3"></span>
            <div>Business</div>
          </label>

          <label class="bg-white flex justify-center items-center flex-col p-6 rounded-xl cursor-pointer shadow">
            <input type="radio" name="category" id="category2" value="personal" v-model="inputCategory" />
            <span class="bubble personal mb-3"></span>
            <div>Personal</div>
          </label>

        </div>

        <input class="w-full mb-5" type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3 class="mb-3 font-semibold">TODO LIST</h3>
      <div class="list flex flex-col gap-3" id="todo-list">

        <div class="flex bg-white items-center justify-between p-3 rounded-2xl shadow" v-for="todo in todosAsc" :key="todo.id"
          :class="`todo-item ${todo.done && 'done'}`">
          <div class="flex gap-3 items-center">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category == 'business'
                ? 'business'
                : 'personal'
                }`"></span>
            </label>

            <div class="todo-content">
              <input class="outline-none text-lg" type="text" v-model="todo.content" />
            </div>
          </div>

          <div class="actions">
            <button class="delete bg-red-500 text-white p-2 rounded-lg hover:bg-red-600"
              @click="removeTodo(todo)">Delete</button>
          </div>
        </div>

      </div>
    </section>

  </main>
</template>

