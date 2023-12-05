<script setup>
import { reactive, onBeforeMount } from "vue";
import { v4 as uuid } from "uuid";
import { getTasks, saveTasks } from "@/libs/storage";
import AddTaskForm from "../components/forms/AddTask.vue";
import TaskItem from "../components/tasks/Item.vue";
import TaskEmpty from "../components/tasks/Empty.vue";

const tasks = reactive([]);

const removeTask = (id) => {
  if (id) {
    const taskIndex = tasks.findIndex((task) => task.id == id);

    if (taskIndex >= 0) {
      tasks.splice(taskIndex, 1);
      saveTasks(tasks);
    }
  }
};

const addTask = (taskData) => {
  const task = {
    ...taskData,
    id: uuid(),
  };

  tasks.unshift(task);

  saveTasks(tasks);
};

onBeforeMount(() => {
  const data = getTasks();
  if (data) {
    tasks.unshift(...data);
  }
});
</script>

<template>
  <main>
    <div class="task-form-area">
      <AddTaskForm @add-task="addTask" />
    </div>
    <div class="task-item-area">
      <div v-if="tasks.length == 0">
        <TaskEmpty />
      </div>
      <div v-else>
        <TaskItem @remove-task="removeTask" v-for="task in tasks" :task="task" />
      </div>
    </div>
  </main>
</template>

<style>
.task-item-area {
  height: 400px;
  overflow-y: auto;
  overflow-x: hidden;
}
</style>
