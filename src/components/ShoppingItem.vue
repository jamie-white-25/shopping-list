<script setup lang="ts">
import { computed } from "vue";

const ctx = defineEmits(["toggleCompleted", "delete"]);

interface itemInterface {
  uuid: string;
  name: string;
  value: number;
  amount: number;
  completed: boolean;
}

const props = defineProps<{
  item: itemInterface | any;
}>();

const item = computed(() => props.item);

const toggleCompleted = () => {
  ctx("toggleCompleted", props.item.uuid, !props.item.completed);
};

const deleteitem = () => {
  ctx("delete", props.item.uuid);
};
</script>

<template>
  <li class="flex space-x-3">
    <div
      @click="toggleCompleted"
      class="px-4 py-2 w-full rounded border border-gray-200 text-sm transition-all duration-300 ease-in-out flex justify-between items-center"
      :class="
        item.completed
          ? 'bg-teal-500 text-white border-teal-500 hover:bg-teal-600 hover:text-gray-200'
          : 'bg-white text-gray-600 border-gray-200 hover:bg-indigo-50 hover:text-gray-700'
      "
    >
      <div class="space-x-2">
        <span class="capitalize">{{ item.name }}</span>
      </div>
      <span>£{{ item.value * item.amount }}</span>
    </div>

    <div
      class="px-4 py-2 w-auto rounded border border-gray-200 text-sm transition-all duration-300 ease-in-out flex justify-between items-center"
      :class="
        item.completed
          ? 'bg-teal-500 text-white border-teal-500 '
          : 'bg-white text-gray-600 border-gray-200 '
      "
    >
      <span>x{{ item.amount }}</span>
    </div>

    <button
      @click="deleteitem"
      class="py-1 px-2 bg-red-500 rounded flex hover:bg-red-700"
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
