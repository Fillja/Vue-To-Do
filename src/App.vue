<script lang="ts" setup>
import { ref } from 'vue';
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';
import type { Task } from './types';

  const message = ref('Tasks App');
  const tasks = ref<Task[]>([]);

  function addTask(newTask: string){
    tasks.value.push({
      id: crypto.randomUUID(),
      title: newTask,
      done: false,
    })
  }

  function toggleDone(taskId: string){
    const task = tasks.value.find((task) => task.id === taskId)

    if(task){
      task.done = !task.done
    }
  }

</script>

<template>
  <main>

    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a task to get started.</h3>
    <h3 v-else> x / {{ tasks.length }} tasks completeted</h3>
    <TaskList @toggle-done="toggleDone" :tasks = tasks />
    
  </main>
</template>


<style scoped>
main{
  max-width: 800px;
  margin: 1rem auto;
}
</style>