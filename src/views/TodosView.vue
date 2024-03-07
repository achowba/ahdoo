<script setup>
import { uid } from 'uid';
import { Icon } from '@iconify/vue';
import { computed, ref, watch } from 'vue';

import TodoItem from '@/components/TodoItem.vue';
import TodoInput from '@/components/TodoInput.vue';

const todos = ref([]);
const TODOS_LOCAL_STORAGE_KEY = 'todos';

watch(todos, () => {
  syncTodosToLocalStorage();
}, {
  deep: true,
});

const allTodosCompleted = computed(() => {
  return todos.value.every((todo) => todo.isCompleted);
});

getTodolistFromLocalStorage();

const createTodo = (_title) => {
  const title = _title.trim();

  if (title.length === 0) {
    return;
  }

  todos.value.push({
    id: uid(),
    title,
    isEditing: false,
    isCompleted: false,
  });
}

const updateTodo = (id, title) => {
  const todo = _findTodoById(id);
  todo.title = title;
}

const deleteTodo = (id) => {
  todos.value = todos.value.filter((_todo) => _todo.id !== id);
}

const toggleTodoComplete = (id) => {
  const todo = _findTodoById(id);

  if (todo.isEditing) {
    return;
  }

  todo.isCompleted = !todo.isCompleted;
}

const toggleTodoEdit = (id) => {
  const todo = _findTodoById(id);

  if (todo.isCompleted) {
    return;
  }

  todo.isEditing = !todo.isEditing;
}

const _findTodoById = (id) => {
  return todos.value.find((_todo) => _todo.id === id);
}

const syncTodosToLocalStorage = () => {
  localStorage.setItem(TODOS_LOCAL_STORAGE_KEY, JSON.stringify(todos.value))
}

function getTodolistFromLocalStorage() {
  let data = localStorage.getItem(TODOS_LOCAL_STORAGE_KEY);

  if (!data) {
    data = '[]';
  }

  todos.value = JSON.parse(data);
}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoInput @todo-create="createTodo" />
    <ul class="todo-list" v-if="todos.length > 0">
      <TodoItem v-for="(todo, index) in todos" :todo="todo" :index="index" @todo-complete="toggleTodoComplete"
        @todo-edit="toggleTodoEdit" @todo-update="updateTodo" @todo-delete="deleteTodo" />
    </ul>
    <p class="todos-message" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no tasks to complete! Add one!</span>
    </p>
    <p v-if="allTodosCompleted && todos.length > 0" class="todos-message">
      <Icon icon="noto-v1:party-popper" width="22" />
      <span>You have completed all your todos</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  max-width: 500px;
  padding: 40px 16px;
  width: 100%;

  ul {
    padding: 0;
  }

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    gap: 20px;
    list-style: none;
    margin-top: 24px;
  }

  .todos-message {
    align-items: center;
    display: flex;
    gap: 8px;
    justify-content: center;
    margin-top: 24px;
  }
}
</style>
