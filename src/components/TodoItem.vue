<script setup lang="ts">
import type { Task } from "../types";

defineProps<{
  task: Task;
}>();
const emit = defineEmits<{
  toggle: [id: number];
  remove: [id: number];
}>();
</script>

<template>
  <input type="checkbox" :checked="task.done" @change="emit('toggle', task.id)" />
  <li>{{ task.text }}</li>
  <button @click="emit('remove', task.id)">Slett</button>
</template>

<style scoped></style>
