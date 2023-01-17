<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const toDos = ref([])
const username = ref('')

const toDoInput = ref('')

onMounted(() => {
  username.value = localStorage.getItem('username') || ''
  toDos.value = JSON.parse(localStorage.getItem('todos')) || []
})

watch(username, (val) => {
  localStorage.setItem('username', val)
})

watch(toDos, val => {
  localStorage.setItem('todos', JSON.stringify(val))
}, { deep: true })

const sortedToDos = computed(() => toDos.value.sort((a, b) => b.createdAt - a.createdAt))

const addToDo = () => {
  const toDo = toDoInput.value
  if(!toDo || toDo.trim() === '') return

  toDos.value.push({
    content: toDo,
    done: false,
    createdAt: Date.now()
  })

  toDoInput.value = ''
}

const removeToDo = (toDo) => {
  toDos.value = toDos.value.filter(t => t !== toDo)
}

</script>

<template>
  <main class="pt-8 flex justify-center">
    <div class="flex-col flex gap-4 w-96">
      <h2 class="text-lg ml-4">Hello,<input class="px-2 py-1 border-none outline-none bg-gray-900 rounded font-bold w-48 " type="text" placeholder="Your name..." v-model="username" /></h2>
      <section class="w-full p-6 bg-gray-800 rounded-lg">
        <h3>Create a TODO:</h3>
        <form class="flex flex-col mt-2" @submit.prevent="addToDo">
          <label class="text-gray-400 text-sm">What's on your toDo list?</label>
          <input
            class="px-2 py-1 rounded mt-1 outline-none bg-gray-600"
            type="text"
            placeholder="Your toDo..."
            v-model="toDoInput"
          />
          <button class="mt-4 bg-orange-700 py-1 px-2 rounded text-gray-100">Add ToDo</button>
        </form>
      </section>  
      <section class="w-full p-6 bg-gray-800 rounded-lg flex flex-col gap-4">
        <h3>TODO list:</h3>
        <div class="flex-col flex gap-2">
          <div v-for="todo in sortedToDos" :class="`flex gap-2 items-center ${todo.done && 'line-through'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
            </label>
            <div class="flex-1">
              <span class="break-all">{{ todo.content }}</span>
            </div>
            <button class="ml-2" @click="removeToDo(todo)">
              <font-awesome-icon class="text-red-600 hover:opacity-60" icon="fa-trash-can"/>
            </button>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>


<style>
body {
	@apply bg-gray-900; color: #fff; padding: 1em;
}
</style>