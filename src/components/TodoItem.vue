<template>
  <li class="todo-item">
    <div class="task-left">
      <input 
        type="checkbox" 
        class="checkbox" 
        v-model="task.completed"
      />
      <span class="task-text" :class="{ completed: task.completed }">
        {{ task.text }}
      </span>
    </div>
    <button class="delete-btn" @click="$emit('remove')">✕</button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  props: ['task'],
}
</script>
