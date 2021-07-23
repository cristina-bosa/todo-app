<template>
    <div class="grid grid-cols-2">
        <div>
            <input type="checkbox" class="inline-block align-middle" :checked='task.active'  @click="$emit('isActive')"/>
            <label @click="$emit('isCompleted')" :class="[task.completed === true ? 'line-through' : 'no-underline']">{{task.title}}</label>
        </div>
        <a v-if="buttonPress=='completed'" @click="$emit('deleteTask')"><span class="material-icons md-18">delete</span></a>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'Task',
  props: {
    task: Object, 
    buttonPress: String,
  },
  emits: ['deleteTask', 'isActive','isCompleted'],
})
</script>
