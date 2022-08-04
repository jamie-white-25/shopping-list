<script setup lang="ts">
import { computed } from "vue";

const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
  index: {
    type: Number,
    required: true,
  },
});

const item = computed(() => props.data);
const index = computed(() => props.index);
</script>

<template>
  <li
    class="flex space-x-3"
    @click="$emit('toggleCompleted', index, !item.completed)"
  >
    <div
      class="px-4 py-2 w-full rounded border border-gray-200 text-sm transition-all duration-300 ease-in-out flex justify-between items-center"
      :class="
        item.completed
          ? 'bg-teal-500 text-white border-teal-500 hover:bg-teal-600 hover:text-gray-200'
          : 'bg-white text-gray-600 border-gray-200 hover:bg-indigo-50 hover:text-gray-700'
      "
    >
      <div class="space-x-2">
        <span class="capitalize">{{ item.name }}</span>
        <span>x{{ item.amount }}</span>
      </div>
      <span>£{{ item.value * item.amount }}</span>
    </div>

    <button
      @click="$emit('delete', index)"
      class="py-1 px-2 bg-red-600 rounded flex hover:bg-red-700"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5 text-white my-auto"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="2"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
        />
      </svg>
    </button>
  </li>
</template>
