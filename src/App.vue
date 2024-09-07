<template>
  <div class="container">
    <h1>Todo List</h1>
    <div class="input-container">
      <input
        v-model="newTodo"
        @keyup.enter="addTodo"
        type="text"
        placeholder="새로운 할 일을 입력하세요"
      />
    </div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <div class="todo-item">
          <input
            type="checkbox"
            v-model="todo.completed"
          />
          <input
            v-if="todo.isEditing"
            v-model="todo.text"
            @blur="finishEdit(todo)"
            @keyup.enter="finishEdit(todo)"
            type="text"
            class="edit-input"
          />
          <span
            v-else
            @dblclick="startEdit(todo)"
            :class="{ 'completed': todo.completed }"
          >
            {{ todo.text }}
          </span>
        </div>
        <div class="todo-actions">
          <button @click="removeTodo(todo.id)" class="delete-btn">삭제</button>
          <button @click="startEdit(todo)" v-if="!todo.isEditing" class="edit-btn">수정</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

interface Todo {
  id: number
  text: string
  completed: boolean
  isEditing: boolean
}

const newTodo = ref('')
const todos = ref<Todo[]>([])

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      completed: false,
      isEditing: false
    })
    newTodo.value = ''
  }
}

const removeTodo = (id: number) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

const startEdit = (todo: Todo) => {
  todo.isEditing = true
}

const finishEdit = (todo: Todo) => {
  todo.isEditing = false
  todo.text = todo.text.trim()
  if (!todo.text) {
    removeTodo(todo.id)
  }
}
</script>

<style>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.input-container {
  margin-bottom: 20px;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
}

.todo-item {
  display: flex;
  align-items: center;
  flex-grow: 1;
  margin-right: 10px;
}

.todo-item input[type="checkbox"] {
  margin-right: 10px;
}

.todo-item span {
  flex-grow: 1;
}

.completed {
  text-decoration: line-through;
}

.edit-input {
  flex-grow: 1;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.todo-actions button {
  margin-left: 10px;
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.delete-btn {
  background-color: #ff4d4d;
  color: white;
}

.edit-btn {
  background-color: #4d94ff;
  color: white;
}
</style>