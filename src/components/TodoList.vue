<script setup lang="ts">
import { ref } from "vue";
import TodoForm from "./TodoForm.vue";
import TodoItem from "./TodoItem.vue";
import TodoStats from "./TodoStats.vue";
import type { Task } from "../types.ts";

const tasks = ref<Task[]>([
  { id: 1, text: "Lage en TODO-liste", done: false },
  { id: 2, text: "Legge til en ny oppgave", done: false },
  { id: 3, text: "Markereen oppgave som ferdig", done: false },
]);

const newTask = ref("");
const filter = ref("");
let nesteId = 3;

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
</script>

<template>
  <section class="todo">
    <h2>Oppgaver</h2>

    <TodoForm v-model="newTask" @add="addTask" />

    <input v-model.trim="filter" placeholder="Søk i oppgaver..." />

    <p v-if="tasks.length === 0">Ingen oppgaver ennå</p>
    <ul v-else>
      <li v-for="task in tasks" :key="task.id">
        <TodoItem :task="task" @toogle="toggleTask" @remove="removeTask" />
      </li>
    </ul>

    <TodoStats :total="tasks.length" :done="tasks.filter((t) => t.done).length" />
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

.row {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}
</style>
