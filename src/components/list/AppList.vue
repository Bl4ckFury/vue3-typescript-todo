<script lang="ts">
import AppListItem from "./AppListItem.vue";
import Todo from "../../models/Item.interface";
import type { PropeType } from "vue";

export default {
  components: {
    AppListItem,
  },
  props: {
    todos: {
      type: Array as PropeType<Todo[]>,
    },
  },
  methods: {
    toggleTodo(id: number) {
      this.$emit("toggleTodo", id);
    },
    removeTodo(id: number) {
      this.$emit("removeTodo", id);
    },
  },
  emits: {
    toggleTodo: (id: number) => Number.isInteger(id),
    removeTodo: (id: number) => Number.isInteger(id),
  },
};
</script>

<template>
  <ul class="todo-list">
    <AppListItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @toggle-todo="toggleTodo"
      @remove-todo="removeTodo"
    />
  </ul>
</template>

<style lang="scss">
@import "../../assets/main.scss";

.todo-list {
  display: flex;
  flex-direction: column;
  justify-content: center;
  list-style: none;
  padding: 0;
}
</style>
