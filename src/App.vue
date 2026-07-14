<script setup>
import {ref, watch} from 'vue'

const todos = ref(JSON.parse(localStorage.getItem('todos')) || [])
const newTodo = ref('')

const addTodo = () => {
  if (newTodo.value.trim() == '') return

  todos.value.push({
    id: Date.now(),
    text: newTodo.value,
    done: false
  })

  newTodo.value = ''
}

const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

const removeAllCeklis = () => {
  todos.value.forEach(todo => {
    todo.done = false
  });
}

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep:true})
</script>

<template>
<div class="todo-app">
  <h1>MY-TODO LIST</h1>

  <form @submit.prevent="addTodo" class="input-form">
    <input type="text" v-model="newTodo" placeholder="Masukkan tugas baru">
    <button type="submit">Tambah</button>
  </form>

  <div v-if="todos.length > 0" class="action-bar">
      <button @click="resetAllCeklis" class="reset-btn">Reset Semua Ceklis</button>
    </div>

  <ul class="todo-list">
    <li v-if="todos.length===0" class="empty-state">
      Belum ada tugas
    </li>

    <li v-for="todo in todos" :key="todo.id" class="todo-item">
      <label :class="{'is-done': todo.done}">
        <input type="checkbox" v-model="todo.done">
        <span>{{ todo.text }}</span>
      </label>
      <button @click="removeTodo(todo.id)" class="delete-btn">Hapus</button>
    </li>
  </ul>
</div>
</template>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  font-family: Arial, sans-serif;
}
h1 { text-align: center; color: #333; }
.input-form { display: flex; gap: 10px; margin-bottom: 20px; }
.input-form input { flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 4px; }
.input-form button { padding: 10px 15px; background: #42b883; color: white; border: none; border-radius: 4px; cursor: pointer; }
.todo-list { list-style: none; padding: 0; }
.todo-item { display: flex; justify-content: space-between; align-items: center; padding: 10px; border-bottom: 1px solid #eee; }
.todo-item label { display: flex; align-items: center; gap: 10px; cursor: pointer; }
.is-done span { text-decoration: line-through; color: #888; }
.delete-btn { background: #ff4d4d; color: white; border: none; padding: 5px 10px; border-radius: 4px; cursor: pointer; }
.empty-state { text-align: center; color: #888; padding: 20px; }
</style>
