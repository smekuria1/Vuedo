<script setup>
import { ref, defineEmits, reactive } from "vue";
import TodoButton from "./TodoButton.vue";
const todoState = reactive({
  todo: "",
  invalid: null,
  errorMsg: "",
});
const emit = defineEmits(["createTodo"]);
const createTodo = () => {
  todoState.invalid = null;
  todoState.errorMsg = "";
  if (todoState.todo !== "") {
    emit("createTodo", todoState.todo);
    todoState.todo = "";
    return;
  }
  todoState.invalid = true;
  todoState.errorMsg = "Todo cannot be empty";
};
</script>

<template>
  <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
    <input
      type="text"
      placeholder="What needs to be done?"
      v-model="todoState.todo"
      @keyup.enter="createTodo"
    />
    <TodoButton @click="createTodo()" />
  </div>
  <p class="err-msg" v-if="todoState.invalid">{{ todoState.errorMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  &.input-err {
    border-color: red;
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}
.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
