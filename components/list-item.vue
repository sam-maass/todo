<template>
  <div class="group bg-white shadow-sm p-4 my-2 hover:shadow-lg transition cursor-pointer flex"
  :class="{'bg-slate-50':item.done}">
    <div class="content flex-1">
      <span
        :class="{
          'line-through text-slate-500': item.done,
        }"
        >{{ item.name }}</span
      >
      <br />
      <span
        class="text-sm text-slate-600"
        :class="{
          'text-slate-400': item.done,
        }"
        >{{ item.createdAt.toLocaleString("de",{dateStyle:"short", timeStyle: "short"}) }}</span
      >
    </div>
    <div class="flex-initial min-h-full grid grid-cols-2 place-items-center">
      <div
        @click="$emit('removeItem', item)"
        class="invisible transition px-2 text-red-200 group-hover:visible hover:text-red-500"
      >
        Delete
      </div>
      <div
        @click="$emit('toggleItem', item)"
        class="rounded-full h-8 w-8 pt-1 text-center"
        :class="{
          'bg-green-400 border-green-400 group-hover:bg-green-500 ': item.done,
          'bg-green-50 border border-green-400 group-hover:bg-green-200': !item.done,
        }"
      >
        <span v-if="item.done">✔️</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { TodoListItem } from "./types";
const emit = defineEmits(["toggleItem","removeItem"]);

const props = defineProps<{
  item: TodoListItem;
}>();
</script>
