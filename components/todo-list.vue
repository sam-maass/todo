<template>
  <div class="py-4">
    <div class="font-sans text-3xl font-bold leading-10 text-center">Todo List</div>
    <div class="py-4 transition">
      <TransitionGroup tag="ListItem" name="list">
        <ListItem
          @toggle-item="toggleItemStatus"
          @remove-item="removeItem"
          v-for="item in list"
          :key="item.name"
          :item="item"
        ></ListItem>
      </TransitionGroup>
    </div>
    <div class="new-item-section flex">
      <div class="flex-1 px-2">
        <input
          v-model="newItem"
          class="leading-8 px-2 w-full h-full outline outline-1 outline-blue-200 hover:outline-blue-600 rounded"
          type="text"
          placeholder="Your next todo"
          @keydown.enter="addListItem"
        />
      </div>
      <button
        class="flex-initial border-blue-600 bg-white border font-c py-2 px-4 rounded"
        @click="addListItem"
      >
        Add Item
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { setupDotenv } from "c12";
import { reactive } from "vue";
import { TodoListItem } from "./types";

const list: TodoListItem[] = reactive([]);
const newItem = ref("");

function addListItem() {
  list.push({
    name: newItem.value,
    done: false,
    createdAt: new Date(),
  });
  newItem.value = "";
  saveToLocalstorage();
}

function toggleItemStatus(item: TodoListItem) {
  const index = getItemIndex(item);
  list[index].done = !list[index].done;
  saveToLocalstorage();
}

function removeItem(item: TodoListItem) {
  const index = getItemIndex(item);
  list.splice(index,1)
  saveToLocalstorage();
}

function getItemIndex(item: TodoListItem) {
  return list.findIndex(
    ({ name, createdAt }) => name==item.name&&createdAt==item.createdAt
  );
}

function saveToLocalstorage() {
  window.localStorage.setItem("todos", JSON.stringify(list));
}

onMounted(() => {
  const items = window.localStorage.getItem("todos") || "[]";
  list.push(...JSON.parse(items));
});
</script>

<style scoped>
.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 150ms ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(-50%) rotateX(90deg)
}
/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
  position: absolute;
}
</style>