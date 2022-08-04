<script setup lang="ts">
import ShoppingItem from "./ShoppingItem.vue";

defineProps({
  items: {
    type: Array,
  },
});

const item = {
  name: String,
  value: Number,
  amount: Number,
};

const ctx = defineEmits(["delete", "toggleCompleted"]);

const deleteItem = (index: number) => {
  ctx("delete", index);
};

const toggleCompleted = (index: number, completed: boolean) => {
  ctx("toggleCompleted", index, completed);
};
</script>

<template>
  <ul class="mb-8 space-y-2">
    <template v-for="(item, i) in items" :key="i">
      <shopping-item
        :item="item"
        :index="i"
        @delete="deleteItem"
        @toggle-completed="toggleCompleted"
      />
    </template>
  </ul>
</template>
