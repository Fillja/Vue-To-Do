<script lang="ts" setup>
import type { Task } from '../types';

    const props = defineProps<{
        tasks: Task[]
    }>();

    const emits = defineEmits<{
        toggleDone: [id: string]
        removeTask: [id: string]
    }>();

</script>

<template>
    <TransitionGroup name="list" tag="div" class="task-list">
        <article class="task-wrapper" v-for="task in props.tasks" :key="task.id">
            <label>
                <input @input="emits('toggleDone', task.id)" :checked="task.done" type="checkbox">
                    <span :class="{ done: task.done }">{{ task.title }}</span>
            </label>
            <button @click="emits('removeTask', task.id)" class="outline">Remove</button>
        </article>
    </TransitionGroup>
</template>


<style>

.task-wrapper{
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.done{
    text-decoration: line-through;
}

.list-enter-active,
.list-leave-active{
    transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to{
    opacity: 0;
    transform: translateX(300px);
}

</style>