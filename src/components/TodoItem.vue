<script setup>
import { Icon } from '@iconify/vue'

const props = defineProps({
  todo: {
    type: Object,
    required: true,
  },
  index: {
    type: Number,
    required: true,
  },
});
defineEmits(['todo-complete', 'todo-edit', 'todo-update', 'todo-delete'])
</script>

<template>
  <li>
    <input type="checkbox" :checked="todo.isCompleted" @input="$emit('todo-complete', todo.id)">
    <div class="todo">
      <input v-if="todo.isEditing" type="text" :value="todo.title" @input="$emit('todo-update', todo.id, $event.target.value)">
      <span v-else :class="{ 'todo-completed': todo.isCompleted }">{{ todo.title }}</span>
    </div>
    <div class="todo-actions">
      <Icon v-if="todo.isEditing" icon="ph:check-circle" class="icon" color="#41b080" width="22"
        @click="$emit('todo-edit', todo.id)" />
      <Icon v-if="!todo.isCompleted" icon="ph:pencil-fill" class="icon" color="#41b080" width="22"
        @click="$emit('todo-edit', todo.id)" />
      <Icon icon="ph:trash" class="icon" color="#f95e5e" width="22" @click="$emit('todo-delete', todo.id)"/>
    </div>
  </li>
</template>

<style lang="scss" scoped>
li {
  align-items: center;
  background-color: #f1f1f1;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  display: flex;
  gap: 10px;
  padding: 16px 10px;

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type="checkbox"] {
    appearance: none;
    background-color: #fff;
    border-radius: 50%;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
    cursor: pointer;
    height: 20px;
    width: 20px;

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;

    .todo-completed {
      color: grey;
      font-weight: 500;
      text-decoration: line-through;
    }

    input[type="text"] {
      border: 2px solid #41b080;
      padding: 2px 6px;
      width: 90%;
    }
  }

  .todo-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;

    .icon {
      cursor: pointer;
    }
  }
}
</style>
