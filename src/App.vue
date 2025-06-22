<script lang="ts" setup>
import { computed, ref } from 'vue';
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';
import FilterButton from './components/FilterButton.vue';
import type { Task, TaskFilter } from './types';

  const message = ref('Tasks App');
  const tasks = ref<Task[]>([]);
  const filter = ref<TaskFilter>("all")

  const totalDone = computed(() => tasks
  .value
  .reduce((total, task) => task.done ? total + 1 : total, 0))

  function addTask(newTask: string){
    tasks.value.push({
      id: crypto.randomUUID(),
      title: newTask,
      done: false,
    })
  }

  const filteredTasks = computed( () => {
    switch(filter.value){
      case "all":
        return tasks.value;

      case "done": 
        return tasks.value.filter((task) => task.done)

      case "todo": 
        return tasks.value.filter((task) => !task.done)
    }
  });

  function toggleDone(taskId: string){
    const task = tasks.value.find((task) => task.id === taskId)

    if(task){
      task.done = !task.done
    }
  }

  function removeTask(taskId: string){
    const taskIndex = tasks.value.findIndex((task) => task.id === taskId);
    if(taskIndex != -1){
      tasks.value.splice(taskIndex, 1);
    }
  }

  function setFilter(filterValue: TaskFilter){
    filter.value = filterValue;
  }

</script>

<template>
  <main>

    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a task to get started.</h3>
    <h3 v-else> {{ totalDone }} / {{ tasks.length }} tasks completeted</h3>
    <div v-if="tasks.length != 0" class="button-container">
      <FilterButton :currentFilter="filter" filter="all" @set-filter="setFilter"/>
      <FilterButton :currentFilter="filter" filter="todo" @set-filter="setFilter"/>
      <FilterButton :currentFilter="filter" filter="done" @set-filter="setFilter"/>
    </div>
    <TaskList @remove-task="removeTask" @toggle-done="toggleDone" :tasks = filteredTasks />
    
  </main>
</template>


<style scoped>
main{
  max-width: 800px;
  margin: 1rem auto;
}

.button-container{
  display: flex;
  justify-content: end;
  gap: .5rem;
  margin-bottom: 1rem;
}
</style>