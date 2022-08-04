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

const deleteItem = (uuid: string) => {
  ctx("delete", uuid);
};

const toggleCompleted = (uuid: string) => {
  ctx("toggleCompleted", uuid);
};
</script>

<template>
  <ul class="mb-8 space-y-2 transition-all duration-300 ease-in-out">
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
