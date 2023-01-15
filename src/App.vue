<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const toDos = ref([])
const username = ref('')

const toDoInput = ref('')

watch(username, (val) => {
  localStorage.setItem('username', val)
})

onMounted(() => {
  username.value = localStorage.getItem('username') || ''
})

const addToDo = () => {
  const toDo = toDoInput.value
  console.log(toDo)
}

</script>

<template>
  <main class="pt-8 flex flex-col items-center">
    <div class="w-fit py-6 px-8 bg-gray-800 rounded-lg">
      <h2 class="text-lg mb-4">Hello,<input class="px-2 py-1 border-none outline-none bg-gray-800 rounded font-bold w-16 min-w-max" type="text" placeholder="Your name..." v-model="username" /></h2>
      <h3>Create a ToDo:</h3>
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
    </div>
  </main>
</template>


<style>
body {
	@apply bg-gray-900; color: #fff;
}
</style>