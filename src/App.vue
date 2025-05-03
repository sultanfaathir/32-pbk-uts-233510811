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
  task.completed == !task.completed
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
  <input type="text" v-model="newTask" @keyup.enter="addTask">
  <button @click="addTask">Tambahkan</button>
  <div>
    <button @click="filter = 'all'">Semua</button>
    <button @click="filter = 'completed'">Selesai</button>
    <button @click="filter = 'active'">Belum Selesai</button>
  </div>

  <ul>
    <li v-for="task in filteredTasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" @change="toggleTask(task)">
      {{ task.text }}
      <button @click="deleteTask(task)">Hapus</button>
    </li>
  </ul>
</template>

<style scoped></style>
