<script setup>
import { defineEmits, reactive } from 'vue';
import TodoButton from './TodoButton.vue';

const emit = defineEmits(['todo-create']);

const todoState = reactive({
  todo: '',
  invalid: false,
  errorMessage: '',
});

const createTodo = () => {
  todoState.invalid = false;
  const title = todoState.todo.trim();

  if (title.length !== 0) {
    emit('todo-create', title);
    todoState.todo = '';
    return;
  }

  todoState.invalid = true;
  todoState.errorMessage = 'Todo title cannot be empty! 😑❌';
};

</script>

<template>
  <div class="input-wrapper" :class="{ 'input-error': todoState.invalid }">
    <input type="text" v-model="todoState.todo" />
    <TodoButton @click="createTodo()"/>
  </div>
  <!-- <p v-if="todoState.invalid" class="error-message">{{ todoState.errorMessage }}</p> -->
  <!-- v-if is more expensive since it actually rewrites the DOM -->
  <p v-show="todoState.invalid" class="error-message">{{ todoState.errorMessage }}</p>
  <br />
</template>

<style lang="scss" scoped>
.input-wrapper {
  border: 2px solid #41b080;
  display: flex;
  transition: 250ms ease;

  &.input-error {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    border: none;
    padding: 8px 6px;
    width: 100%;

    &:focus {
      outline: none;
    }
  }
}

.error-message {
  color: red;
  font-size: 1.2rem;
  margin-top: 6px;
  text-align: center;
}
</style>
