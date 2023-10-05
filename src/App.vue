<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const todos = ref([])
const name = ref('')

const inputContent = ref('')
const inputCategory = ref(null)

const todoAsc = computed(() => {
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt
  })
})

const addTodo = () => {

}

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})

</script>

<template>
  <main class="app">
    <div class="container">
      <section class="greeting mb-5">
        <h2 class="title text-lg font-semibold">
          What's up? <input class="outline-none inline-block w-2/3 bg-transparent" type="text" v-model="name"
            placeholder="Name here">
        </h2>
      </section>

      <section class="create-todos">
        <h3 class="text-base text-gray-700">CREATE A TODO</h3>

        <form @submit.prevent="addTodo">
          <h4 class="text-gray-400 mb-5">What's on your todo list?</h4>
          <input class="outline-none border-2 rounded w-full p-2 mb-4" type="text" placeholder="e.g. make a video"
            v-model="inputContent">
          <h4 class="text-gray-700 mb-3">Pick a category</h4>

          <div class="options flex justify-between gap-3">
            <label class="bg-slate-100 w-1/2 p-2 flex justify-center items-center flex-col rounded h-[80px]">
              <input type="radio" name="category" value="business" v-model="inputCategory">
              <span class="buble"></span>
              <div>
                Business
              </div>
            </label>

            <label class="bg-slate-100 w-1/2 p-2 flex justify-center items-center flex-col rounded h-[80px]">
              <input type="radio" name="category" value="personal" v-model="inputCategory">
              <span class="buble"></span>
              <div>
                Personal
              </div>
            </label>
          </div>
        </form>
      </section>
    </div>
  </main>
</template>