<template>
  <div :class="[todo.completed ? 'completed' : '', 'todo']">
    <span>{{ todo.title }}</span>
    <div class="icons">
      <Edit class="svg-icon" @edit="emit('edit')" />
      <Delete class="svg-icon" @delete="emit('delete')" />
      <input
        type="checkbox"
        class="checkbox"
        @input="emit('complete')"
        :checked="todo.completed"
      />
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'
import Edit from '../assets/edit-icon.vue'
import Delete from '../assets/delete-icon.vue'

defineProps({
  todo: Object,
})

const emit = defineEmits(['edit', 'delete', 'complete'])
</script>

<style scoped>
.todo {
  text-align: left;
  width: 40%;
  font-size: 1.3rem;
  padding: 15px;
  margin: 10px;
  border: 1px solid white;
  color: white;
  display: flex;
  justify-content: space-between;
}
.completed {
  text-decoration: line-through;
  opacity: 0.7;
}
.checkbox {
  width: 23px;
  height: 23px;
}
.svg-icon {
  cursor: pointer;
}

.icons {
  display: flex;
  gap: 10px;
  align-items: center;
}
</style>
