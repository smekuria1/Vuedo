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
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-if="todoList.length === 0">
      <span>👋</span>
      <span>Start creating your todos</span>
    </p>
    <p class="todos-msg" v-else-if="todoCompleted">
      <span>🎉</span>
      <span>Great job! You completed all your todos</span>
    </p>
  </main>
</template>

<script setup>
import TodoCreate from "../components/TodoCreate.vue";
import { ref, watch, computed } from "vue";
import { uid } from "uid";
import TodoItem from "../components/TodoItem.vue";
const todoList = ref([]);
watch(
  todoList,
  (val) => {
    setTodoListLocalStorage();
  },
  {
    deep: true,
  }
);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted === true);
});

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};
fetchTodoList();
const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });
  setTodoListLocalStorage();
};
const toggleTodoComplete = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
  setTodoListLocalStorage();
};
const toggleEditTodo = (index) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing;
  setTodoListLocalStorage();
};
const updateTodo = (todo, index) => {
  todoList.value[index].todo = todo;
  setTodoListLocalStorage();
};
const deleteTodo = (id) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== id);
  setTodoListLocalStorage();
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
