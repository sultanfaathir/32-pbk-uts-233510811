<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filter = ref('all')

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const toggleTask = (task) => {
  task.completed = !task.completed
}

const deleteTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const filteredTasks = computed(() => {
  if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else if (filter.value === 'active') {
    return tasks.value.filter(task => !task.completed)
  } else {
    return tasks.value
  }
})
</script>

<template>
  <div class="h-screen w-screen overflow-hidden flex flex-col md:flex-row bg-gradient-to-br from-blue-50 to-white text-gray-800 transition-all duration-500 ease-in-out">
    
    <!-- Daftar Tugas -->
    <div class="md:w-2/3 w-full flex flex-col px-4 pt-4 pb-2 md:px-10 md:pt-10 md:pb-0 h-1/2 md:h-full">
      <h1 class="text-xl md:text-2xl font-bold mb-2 md:mb-4 text-blue-900">Daftar Tugas</h1>
      <ul class="space-y-2 overflow-y-auto pr-1 md:pr-2 flex-1">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          class="flex justify-between items-center bg-white p-3 rounded-md shadow-sm transition hover:shadow-md animate-fade-in"
        >
          <div class="flex items-center space-x-2">
            <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" class="accent-blue-600 w-4 h-4" />
            <span :class="{ 'line-through text-gray-400': task.completed }" class="text-sm md:text-base">
              {{ task.text }}
            </span>
          </div>
          <button @click="deleteTask(task)" class="text-xs text-red-500 hover:underline">Hapus</button>
        </li>
      </ul>
    </div>

    <!-- Sidebar -->
    <div class="md:w-1/3 w-full bg-white rounded-t-xl md:rounded-none md:rounded-l-xl shadow-lg px-4 py-4 md:px-6 md:py-8 animate-slide-in h-1/2 md:h-full flex flex-col justify-between">
      <div>
        <h2 class="text-lg md:text-xl font-semibold mb-3 text-blue-800">Tambah Tugas</h2>
        <input
          type="text"
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="Tugas baru..."
          class="w-full px-3 py-2 rounded-md border border-gray-300 focus:ring-2 focus:ring-blue-400 focus:outline-none mb-3 text-sm"
        />
        <button
          @click="addTask"
          class="w-full bg-blue-600 text-white py-2 rounded-md hover:bg-blue-700 transition text-sm"
        >
          Tambahkan
        </button>
      </div>

      <div class="mt-4 md:mt-6">
        <h3 class="mb-2 font-semibold text-gray-700 text-sm">Filter</h3>
        <div class="flex justify-between gap-2">
          <button
            @click="filter = 'all'"
            :class="['w-full py-2 rounded-md text-sm transition', filter === 'all' ? 'bg-blue-100 text-blue-800' : 'bg-gray-100 hover:bg-gray-200']"
          >
            Semua
          </button>
          <button
            @click="filter = 'completed'"
            :class="['w-full py-2 rounded-md text-sm transition', filter === 'completed' ? 'bg-blue-100 text-blue-800' : 'bg-gray-100 hover:bg-gray-200']"
          >
            Selesai
          </button>
          <button
            @click="filter = 'active'"
            :class="['w-full py-2 rounded-md text-sm transition', filter === 'active' ? 'bg-blue-100 text-blue-800' : 'bg-gray-100 hover:bg-gray-200']"
          >
            Belum
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slide-in {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.animate-fade-in {
  animation: fade-in 0.4s ease-out;
}

.animate-slide-in {
  animation: slide-in 0.4s ease-out;
}
</style>
