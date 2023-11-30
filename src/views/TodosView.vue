<script setup lang="ts">
import { Icon } from "@iconify/vue";
import TodoCreator from "../components/TodoCreator.vue";
import { uid } from "uid";
import { ref } from "vue";
import type TodoItemType from "../types/todo.types";
import TodoItem from "../components/TodoItem.vue";

const todoList = ref<TodoItemType[]>([]);

const createTodo = (todo: string) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false,
  });
};

const toggleTodoComplete = (todoPosition: number) => {
  todoList.value[todoPosition].isCompleted =
    !todoList.value[todoPosition].isCompleted;
};

const toggleEditTodo = (todoPosition: number) => {
  todoList.value[todoPosition].isEditing =
    !todoList.value[todoPosition].isEditing;
}

const updateTodo = (todoValue: string, todoPosition: number) => {
  todoList.value[todoPosition].todo = todoValue
}

const deleteTodo = (todoId: string) => {
  todoList.value =   todoList.value.filter((todo) => todo.id !== todoId)
}

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
  </main>
</template>

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
