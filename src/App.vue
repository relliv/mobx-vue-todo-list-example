<template>
  <Observer>
    <main class="flex flex-col gap-3 p-5">
      <!-- Header -->
      <header>
        My Todo List
        <span v-if="todoStore.isAllComplete">- All Done 🎉</span>
        <span v-else>(Left: {{ todoStore.uncompletedItems }})</span>
      </header>

      <!-- Main Content -->
      <section>
        <ul class="flex flex-col gap-2">
          <li
            v-for="(todo, index) in todoStore.filteredTodos"
            :key="index"
            :class="{ '!bg-yellow-500': todo.completed }"
            class="flex flex-row justify-between border border-gray-200 px-2 py-1 rounded-sm dark:hover:bg-gray-700"
          >
            <div class="flex flex-row gap-2">
              <input class="toggle" type="checkbox" v-model="todo.completed" />
              <label>{{ todo.title }}</label>
            </div>

            <button
              class="bg-rose-600 hover:bg-rose-500 px-3 text-xs rounded-[4px] justify-end text-white"
              @click="todoStore.removeTodo(todo)"
            >
              Remove
            </button>
          </li>
          <h1
            v-if="todoStore.filteredTodos.length === 0"
            class="bg-cyan-700 px-3 py-2 text-sm rounded-sm"
          >
            No Todo Item
          </h1>
        </ul>
      </section>

      <!-- Form -->
      <section>
        <div class="flex flex-row gap-2 w-full">
          <input
            v-model="newTodo"
            class="rounded-md px-2 py-2 text-xs dark:text-gray-800 w-full border border-gray-400 dark:bg-gray-700 dark:text-white"
            placeholder="todo"
          />
          <button
            class="px-2 py-1 bg-yellow-500 rounded-md dark:text-gray-700 text-xs"
            @click="onAddClick"
          >
            Add
          </button>
        </div>
      </section>

      <!-- Other Actions -->
      <footer>
        <div class="flex flex-row gap-1">
          <input
            id="toggle-all"
            class="toggle-all"
            type="checkbox"
            v-model="todoStore.isAllComplete"
            @change="todoStore.setCompleteAll(!todoStore.isAllComplete)"
          />
          <label for="toggle-all">Mark all as complete</label>
        </div>
      </footer>
    </main>
  </Observer>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { TodoStore } from './stores/todo.store';
import { ITodo } from './model/todo.model';
import { Observer } from 'mobx-vue-lite';

const newTodo = ref('');
const todoStore = TodoStore;

const initialTodos = [
  { title: 'Wake up at', completed: false },
  { title: 'Brush teeth', completed: false },
  { title: 'Take a shower', completed: false },
  { title: 'Get dressed', completed: false },
  { title: 'Eat breakfast', completed: true },
];

if (!todoStore.todos.length) {
  initialTodos.forEach((todo: ITodo) => todoStore.addTodo(todo));
}

const onAddClick = () => {
  if (newTodo.value.trim()) {
    todoStore.addTodo({
      title: newTodo.value.trim(),
      completed: false,
    });
    newTodo.value = '';
  }
};
</script>

<style scoped></style>
