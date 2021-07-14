<template>
  <todo-list @addTodo="addTodo" />
  <div class="max-w-md bg-white rounded-lg p-4 mx-auto" v-if="todosLength">
    <transition-group tag="ul" appear mode="out-in" name="list" class="flex flex-col space-y-2">
      <li
        class="
          p-4
          border-2
          border-gray-100
          flex
          justify-between
          items-center
        "
        v-for="todo in todos"
        :key="todo.id"
      >
        <h1
          :class="{ 'line-through': todo.done }"
          @click="markDone(todo)"
          class="text-blue-500 cursor-pointer"
        >
          {{ todo.name }}
        </h1>
        <div class="flex justify-between space-x-2">
          <button
            @click="openModal(todo)"
            class="bg-red-500 p-2 rounded text-white"
          >
            Remove
          </button>
        </div>
      </li>
    </transition-group>
  </div>
  <div
    v-else
    class="max-w-md bg-white rounded-lg mx-auto p-4 text-center text-green-500"
  >
    Nothing in todos!
  </div>
  <transition name="modal">
    <delete-todo
      v-if="deleteModalIsVisible"
      :todo="selectedTodo"
      @removeTodo="remove"
      @cancel="cancel"
    ></delete-todo>
  </transition>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { computed, onMounted } from "@vue/runtime-core";
import axios from "axios";
import TodoList from "./AddTodo.vue";
import DeleteTodo from "./DeleteTodo.vue";

const todos = ref([]);
const deleteModalIsVisible = ref(false);
const selectedTodo = ref(null);
const todosLength = computed(() => todos.value.length);
onMounted(() => {
  fetchTodos();
});

async function fetchTodos() {
  const res = await axios.get("http://localhost:8000/todos");
  todos.value = res.data;
}

async function addTodo(todo) {
  const res = await axios.post("http://localhost:8000/todos", todo);

  if (res.status === 201) {
    fetchTodos();
  } else {
    console.log(res);
  }
}

async function markDone(todo) {
  todo.done = !todo.done;
  const res = await axios.put("http://localhost:8000/todos/" + todo.id, todo);
  if (res.status === 200) {
    fetchTodos();
  } else {
    console.log(res);
  }
}

function openModal(todo) {
  selectedTodo.value = todo;
  deleteModalIsVisible.value = true;
}

async function remove(todoId) {
  const res = await axios.delete("http://localhost:8000/todos/" + todoId);
  deleteModalIsVisible.value = false;
  fetchTodos();
}

function cancel() {
  deleteModalIsVisible.value = false;
}
</script>

<style scoped>
.modal-enter-from {
  opacity: 0.6;
  transform: translateY(-60%);
}
.modal-enter-active {
  transition: all 0.3s ease-in-out;
}

.modal-leave-to {
  opacity: 0.6;
  transform: translateY(-60%);
}
.modal-leave-active {
  transition: all 0.3s ease-in-out;
}

.list-enter-from {
  opacity: 0.4;
  transform: scale(0.4);
}
.list-enter-active {
  transition: all ease-in-out;
}

.list-leave-to {
  opacity: 0.4;
  transform: scale(0.4);
}
.list-leave-active {
  transition: all 0.3s ease-in-out;
}
</style>
