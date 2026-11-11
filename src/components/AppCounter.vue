<script setup lang="ts">
import { ref } from "vue";

const count = ref(0);
const countTwo = ref(0);
const addBox = ref("");

function increment() {
  count.value++;
}

function incrementTwo() {
  countTwo.value++;
}

function addSomething() {
  addBox.value = "You added this text!";
}
</script>

<template>
  <div class="mainStyle">
    <p>Teller One: {{ count }}</p>
    <button @click="increment">+1</button>

    <p>Add: {{ addBox }}</p>
    <button @click="addSomething">Add</button>

    <p>Teller Two: {{ countTwo }}</p>
    <button @click="incrementTwo">+1</button>
  </div>
</template>

<style scoped>
.mainStyle {
  color: hotpink;
  font-size: x-large;
}
</style>
