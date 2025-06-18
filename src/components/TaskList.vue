<script lang="ts" setup>
import type { Task } from '../types';

    const props = defineProps<{
        tasks: Task[]
    }>();

    const emits = defineEmits<{
        toggleDone: [id: string]
    }>();

</script>

<template>
    <article v-for="task in props.tasks" :key="task.id">
        <label>
            <input @input="emits('toggleDone', task.id)" :checked="task.done" type="checkbox">
                {{ task.title }}
                {{ task.done }}
        </label>
    </article>
</template>