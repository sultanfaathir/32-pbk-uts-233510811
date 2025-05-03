<script setup>
import { ref } from 'vue'

const tasks = ref([])
const newTask = ref('')

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

</script>

<template>
  <input type="text" v-model="newTask" @keyup.enter="addTask">
  <button @click="addTask">Tambahkan</button>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" @change="toggleTask(task)">
      {{ task.text }}
      <button @click="deleteTask(task)">Hapus</button>
    </li>
  </ul>
</template>

<style scoped></style>
