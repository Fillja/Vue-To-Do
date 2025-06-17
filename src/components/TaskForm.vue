<script lang="ts" setup>
    import { ref } from 'vue';

    const emit = defineEmits<{
        addTask: [newTask:string]
    }>();

    const newTask = ref("");
    const error = ref("");

    function formSubmitted(){
        if(newTask.value.trim()){
            emit("addTask", newTask.value.trim());
            newTask.value = "";
        }
        else{
            error.value = "Task cannot be empty.";
        }
    }

</script>

<template>
    <form @submit.prevent="formSubmitted">
      <label>
        New Tasks
        <input 
            :aria-invalid="!!error || undefined" 
            v-model="newTask" 
            name="newTask"
            @input="error = ''"
        >
      </label>

      <small v-if="error" id="invalid-helper">
        {{ error }}
      </small>

      <div class="button-container">
        <button>Add</button>
      </div>

    </form>
</template>

<style scoped>
.button-container{
  display: flex;
  justify-content: end;
}
</style>