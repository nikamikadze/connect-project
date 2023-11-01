<template>
  <svg @click="emit('delete')" width="24" height="24" viewBox="0 0 48 48">
    <path
      fill="#F44336"
      d="M21.5 4.5H26.501V43.5H21.5z"
      transform="rotate(45.001 24 24)"
    ></path>
    <path
      fill="#F44336"
      d="M21.5 4.5H26.5V43.501H21.5z"
      transform="rotate(135.008 24 24)"
    ></path>
  </svg>
</template>

<script setup>
import { defineEmits } from 'vue'
const emit = defineEmits(['delete'])
</script>
