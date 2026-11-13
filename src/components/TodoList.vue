<script setup lang="ts">
import { ref } from "vue";
import TodoForm from "./TodoForm.vue";
import TodoItem from "./TodoItem.vue";
import TodoStats from "./TodoStats.vue";
import type { Task } from "../types.ts";
import AppModal from "./AppModal.vue";

const tasks = ref<Task[]>([
  { id: 1, text: "Faste til solnedgang", done: false },
  { id: 2, text: "Legge til en ny oppgave", done: false },
  { id: 3, text: "Markere en oppgave som ferdig", done: false },
]);

const newTask = ref("");
const isDeleteOpen = ref(false);
const isAboutOpen = ref(false);
const taskToDelete = ref<number | null>(null);
const filter = ref("");
let nesteId = 4;

function addTask() {
  const text = newTask.value.trim();
  if (!text) return;

  tasks.value.push({ id: nesteId++, text, done: false });
  newTask.value = "";
}
function toggleTask(id: number) {
  const task = tasks.value.find((t) => t.id === id);
  if (task) task.done = !task.done;
}
function removeTask(id: number) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
}

function askDelete(id: number) {
  taskToDelete.value = id;
  isDeleteOpen.value = true;
}

function confirmDelete() {
  if (taskToDelete.value !== null) removeTask(taskToDelete.value);
  isDeleteOpen.value = false;
}

function showAbout() {
  isAboutOpen.value = true;
}
</script>

<template>
  <button @click="showAbout">Om appen</button>
  <section class="todo">
    <h2>Oppgaver</h2>

    <TodoForm v-model="newTask" @add="addTask" />

    <input v-model.trim="filter" placeholder="Søk i oppgaver..." />

    <p v-if="tasks.length === 0">Ingen oppgaver ennå</p>
    <ul v-else>
      <li v-for="task in tasks" :key="task.id">
        <TodoItem :task="task" @toggle="toggleTask" @remove="askDelete" />
      </li>
    </ul>

    <TodoStats :total="tasks.length" :done="tasks.filter((t) => t.done).length" />
    <AppModal v-if="isDeleteOpen" @close="isDeleteOpen = false">
      <h2>Er du sikker?</h2>
      <p>{{ tasks.find((t) => t.id == taskToDelete)?.text }} blir slettet permantent</p>
      <button @click="confirmDelete">Ja, slett</button>
    </AppModal>
    <AppModal v-if="isAboutOpen" @close="isAboutOpen = false">
      <h2>Todo List App</h2>
      <p>Flott app, der du kan registrere dine gjøremål.</p>
    </AppModal>
  </section>
</template>

<style scoped>
.todo {
  max-width: 460px;
}
.done span {
  text-decoration: line-through;
  color: hotpink;
}

li {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  margin-bottom: 0.5rem;
}
</style>
