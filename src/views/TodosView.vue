<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreate @create-todo="createTodo" />
    <ul class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
      />
    </ul>
    <p class="todos-msg" v-if="todoList.length === 0">
      <span>👋</span>
      <span>Start creating your todos</span>
    </p>
  </main>
</template>

<script setup>
import TodoCreate from "../components/TodoCreate.vue";
import { ref } from "vue";
import { uid } from "uid";
import TodoItem from "../components/TodoItem.vue";
const todoList = ref([]);
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });
};
const toggleTodoComplete = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
};
</script>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
