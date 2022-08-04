<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import { v4 as uuidv4 } from "uuid";
import ShoppingList from "./components/ShoppingList.vue";

onMounted(() => {
  const storage = localStorage.getItem("shoppingList");
  if (storage) {
    list.value = JSON.parse(storage);
  }
});

const total = computed(() => {
  let sum = 0;
  list.value.forEach((item) => {
    sum = sum + item.value * item.amount;
  });
  return sum;
});

const completedTotal = computed(() => {
  let sum = 0;
  list.value.forEach((item) => {
    if (item.completed) {
      sum = sum + item.value * item.amount;
    }
  });
  return sum;
});

const makeUuid = () => {
  let uuid = uuidv4();
  console.log(uuid);
  return uuid;
};

const additem = () => {
  if (validate()) {
    item.value.uuid = makeUuid();
    list.value.push({ ...item.value });
    item.value.name = "";
    item.value.amount = 0;
    item.value.value = 0;
  }

  setStorage();
};

const deleteAll = () => {
  list.value = [];
};

const deleteItem = (uuid: string) => {
  list.value = list.value.filter((item) => {
    return item.uuid !== uuid;
  });
  setStorage();
};

const toggleCompleted = (uuid: string) => {
  console.log(uuid);
  list.value.forEach((item) => {
    if (item.uuid === uuid) {
      item.completed = !item.completed;
    }
  });

  setStorage();
};

const validate = () => {
  if (!item.value.name) {
    return false;
  }

  if (isNaN(item.value.amount)) {
    return false;
  }

  if (isNaN(item.value.value)) {
    return false;
  }

  return true;
};

const item = ref({
  uuid: "",
  name: "",
  value: 0,
  amount: 1,
  completed: false,
});

const list = ref([
  { uuid: makeUuid(), name: "Test", value: 20, amount: 1, completed: false },
]);

const setStorage = () => {
  localStorage.setItem("shoppingList", JSON.stringify(list.value));
};
</script>

<template>
  <main class="">
    <div
      class="absolute top-0 right-0 h-screen w-screen bg-gradient-to-r from-[#8EC5FC] to-[#E0C3FC]"
    ></div>

    <div class="relative mt-16 z-10 bg-none max-w-2xl mx-auto">
      <div
        class="bg-indigo-300 bg-opacity-25 px-6 pt-8 pb-5 rounded-md border border-indigo-200 m-2"
      >
        <div v-if="list.length >= 1" class="flex items-center justify-end mb-6">
          <button
            @click="deleteAll"
            type="button"
            class="inline-flex items-center p-2 border border-red-500 rounded shadow-sm text-white bg-red-500 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
          >
            Delete all
          </button>
        </div>

        <ShoppingList
          :items="list"
          @delete="deleteItem"
          @toggle-completed="toggleCompleted"
        />
      </div>

      <div
        class="bg-none py-2 space-y-4 md:space-y-0 md:space-x-4 rounded flex flex-col md:flex-row justify-evenly align-baseline mt-20 md:mt-6 mx-8"
      >
        <div>
          <label
            for="name"
            class="block text-sm font-medium text-gray-800 mb-1"
          >
            Name
          </label>
          <input
            v-model="item.name"
            type="text"
            id="name"
            required
            class="shadow-sm focus:ring-indigo-400 focus:border-indigo-400 block w-full sm:text-sm border-gray-300 rounded-md text-gray-600"
            placeholder="name"
          />
        </div>
        <div>
          <label
            for="value"
            class="block text-sm font-medium text-gray-800 mb-1"
          >
            value
          </label>
          <input
            v-model.number="item.value"
            type="number"
            id="value"
            required
            placeholder="1"
            class="shadow-sm focus:ring-indigo-400 focus:border-indigo-400 block w-full sm:text-sm border-gray-300 rounded-md text-gray-600"
          />
        </div>
        <div>
          <label
            for="amount"
            class="block text-sm font-medium text-gray-800 mb-1"
          >
            Quantity
          </label>
          <input
            v-model.number="item.amount"
            type="number"
            id="amount"
            required
            placeholder="1"
            class="shadow-sm focus:ring-indigo-400 focus:border-indigo-400 block w-full sm:text-sm border-gray-300 rounded-md text-gray-600"
          />
        </div>
      </div>

      <div class="flex justify-end flex-col mt-6 px-2 mx-8">
        <span class="text-gray-700 font-semibold text-md ml-auto pb-1">
          Total: {{ total }}
        </span>
        <hr class="w-1/3 ml-auto border-gray-50 border-1" />
        <span class="text-gray-700 font-semibold text-md ml-auto pt-1">
          Completed Total: {{ completedTotal }}
        </span>
      </div>

      <div class="flex justify-end mt-8 mx-8">
        <button
          @click="additem"
          type="button"
          class="inline-flex items-center p-3 border border-gray-300 rounded-full shadow-sm text-white bg-pink-400 hover:bg-pink-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          <!-- Heroicon name: outline/plus-sm -->
          <svg
            class="h-8 w-8"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="2"
            stroke="currentColor"
            aria-hidden="true"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 6v6m0 0v6m0-6h6m-6 0H6"
            />
          </svg>
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
