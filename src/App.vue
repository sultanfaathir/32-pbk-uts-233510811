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

</script>

<template>
 <input type="text" v-model="newTask" @keyup.enter="addTask">
 <button @click="addTask">Tambahkan</button> 
</template>

<style scoped>

</style>
