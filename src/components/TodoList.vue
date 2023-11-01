<template>
  <form @submit.prevent="submitHandler" class="todo-form">
    <div class="todo-input-container">
      <input
        type="text"
        ref="todoInput"
        v-model="TodoTitle"
        required
        class="todo-input"
      />

      <Edit v-if="isEditing" @edit="changeTitle" />
      <Plus v-else @create="createTodo" type="submit" />
    </div>
  </form>

  <div v-for="todo in TodoList" :key="todo.id" class="todo-list">
    <TodoItem
      :todo="todo"
      @edit="editTodo(todo)"
      @delete="deleteTodo(todo)"
      @complete="completeTodo(todo)"
    />
  </div>
</template>

<script setup>
import TodoItem from './TodoItem.vue'
import { onMounted, ref } from 'vue'
import axios from 'axios'
import Edit from '../assets/edit-icon.vue'
import Plus from '../assets/plus-icon.vue'

const TodoTitle = ref('')
const TodoList = ref([])
const isEditing = ref(false)
const whichIsEditing = ref({})
const todoInput = ref(null)

function submitHandler() {
  if (isEditing.value) {
    changeTitle()
  } else {
    createTodo()
  }
}

function completeTodo(todo) {
  TodoList.value = TodoList.value.map((t) => {
    if (t === todo) {
      return { ...t, completed: !t.completed }
    }
    return t
  })
}

function editTodo(todo) {
  TodoTitle.value = todo.title
  isEditing.value = true
  whichIsEditing.value = todo
  todoInput.value.focus()
}

function changeTitle() {
  TodoList.value = TodoList.value.map((t) => {
    if (t === whichIsEditing.value) {
      return { ...t, title: TodoTitle.value }
    } else {
      return t
    }
  })
  TodoTitle.value = ''
  isEditing.value = false
  whichIsEditing.value = {}
}

function deleteTodo(todo) {
  TodoList.value = TodoList.value.filter((t) => t !== todo)
}

async function createTodo() {
  const response = await axios.post(
    'https://jsonplaceholder.typicode.com/todos'
  )

  TodoList.value.unshift({
    title: TodoTitle.value,
    id: response.id,
    completed: false,
  })
  TodoTitle.value = ''
}

onMounted(async () => {
  const response = await axios.get('https://jsonplaceholder.typicode.com/todos')
  TodoList.value = response.data
})
</script>

<style scoped>
.todo-form {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.todo-input-container {
  background-color: black;
  border: 3px solid white;
  display: flex;
  align-items: center;
  gap: 10px;
  border-radius: 12px;
  margin-bottom: 30px;
  padding-right: 10px;
  box-shadow: 0 0 10px white;
}
.todo-input {
  font-size: 1.3rem;
  padding: 10px;
  background: none;
  border: none;
  outline: none;
  color: white;
}

.todo-list {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
