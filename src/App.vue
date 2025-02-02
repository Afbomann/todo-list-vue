<script setup lang="ts">
import { onMounted, ref } from "vue";

const input = ref<string>("");
const tasks = ref<string[]>([]);

const addTask = () => {
  if (!input.value.trim()) return;

  tasks.value.unshift(input.value);
  input.value = "";
  saveTasks();
};

const deleteTask = (taskIndex: number) => {
  tasks.value.splice(taskIndex, 1);
  saveTasks();
};

const loadTasks = () => {
  const data = localStorage.getItem("tasks");

  if (!data) return;

  const parsed = JSON.parse(data);

  if (!Array.isArray(parsed) || !parsed.every((item) => typeof item == "string")) return;

  tasks.value = parsed;
};

const saveTasks = () => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

onMounted(() => loadTasks());
</script>

<template>
  <div class="w-[600px] max-w-[85%] mx-auto my-[5dvh]">
    <h2 class="text-xl lg:text-2xl">Todo List</h2>

    <form v-on:submit.prevent="addTask()" class="mt-[2dvh] flex gap-[2dvh]">
      <input
        class="flex-1 bg-slate-200 px-[10px] py-[5px] rounded-lg outline-none text-sm lg:text-base"
        placeholder="Enter your task.."
        v-model="input"
        type="text"
      />
      <input
        type="submit"
        value="Add task"
        class="bg-green-400 px-[15px] py-[5px] rounded-lg cursor-pointer text-sm lg:text-base"
      />
    </form>

    <div class="mt-[3dvh] flex flex-col gap-[2dvh]">
      <div
        class="bg-slate-200 rounded-lg p-[12px] flex gap-[2dvh] items-start"
        v-for="(task, taskIndex) in tasks"
        v-bind:key="task"
      >
        <p class="text-sm lg:text-base">{{ task }}</p>
        <button
          class="bg-red-400 rounded-lg text-base lg:text-lg ml-auto px-[10px] py-[3px] cursor-pointer"
          v-on:click="deleteTask(taskIndex)"
        >
          X
        </button>
      </div>
    </div>
  </div>
</template>
