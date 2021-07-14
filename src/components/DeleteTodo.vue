<template>
  <div class="fixed top-0 left-0 w-full">
    <div
      class="
        max-w-md
        bg-white
        rounded-lg
        border-2 border-gray-200
        mx-auto
        my-10
        flex flex-col
      "
    >
      <div class="p-4 border-b text-red-500">
        Are you sure to remove this item, {{ todo.name }}?
      </div>
      <div class="flex justify-around p-4">
        <button
          @click="emit('removeTodo', todo.id)"
          class="bg-red-500 p-2 rounded"
        >
          Confirm
        </button>
        <button @click="emit('cancel')" class="bg-yellow-500 p-2 rounded">
          Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps(["todo"]);
const emit = defineEmits(["removeTodo", "cancel"]);
</script>

<style></style>
