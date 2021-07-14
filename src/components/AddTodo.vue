<template>
  <div class="max-w-md mx-auto py-4 flex justify-center items-center space-x-2 mb-10 bg-white rounded-lg">
    <input
      @keyup.enter="addTodo"
      type="text"
      placeholder="Enter Todo Name"
      class="p-2 border-0 rounded ring-2 ring-gray-200 focus:ring-green-500"
      v-model="todo.name"
    />
    <button @click="addTodo" class="p-2 bg-green-500 rounded text-white">Add</button>
  </div>
</template>

<script setup>
import { reactive } from "@vue/reactivity";

const todo = reactive({
  name: "",
  done: false,
});

const emit = defineEmits(["addTodo"]);

function addTodo() {
  if (!todo.name) return;
  emit("addTodo", todo);
  todo.name = "";
}
</script>

<style></style>
