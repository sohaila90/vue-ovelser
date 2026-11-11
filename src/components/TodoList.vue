<script setup lang="ts">
import { ref } from "vue";

type Task = {
  id: number;
  text: string;
  done: boolean;
  comment?: string;
};

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
function toggleTask(task: Task) {
  task.done = !task.done;
}
function removeTask(id: number) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
}
</script>

<template>
  <section class="todo">
    <h2>Oppgaver</h2>

    <form @submit.prevent="addTask">
      <input @keyup.enter="addTask" v-model.trim="newTask" placeholder="Skriv en oppgave..." />
      <button type="submit">Legg til</button>
    </form>

    <input v-model.trim="filter" placeholder="Søk i oppgaver..." />

    <p v-if="tasks.length === 0">Ingen oppgaver ennå</p>
    <ul v-else>
      <li
        v-for="task in tasks.filter((t) => t.text.toLowerCase().includes(filter.toLowerCase()))"
        :key="task.id"
        :class="{ done: task.done }"
      >
        <div class="row">
          <input type="checkbox" :checked="task.done" @change="toggleTask(task)" />
          <span>{{ task.text }}</span>
          <button @click="removeTask(task.id)">Slett</button>
        </div>
        <input v-model.lazy="task.comment" placeholder="Kommentar (valgfritt)" />
      </li>
    </ul>
    <p>{{ tasks.length }} oppgaver, {{ tasks.filter((t) => t.done).length }} ferdig</p>
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
