<script lang="ts">
import Todo from "./models/Item.interface";
import Filter from "./models/Filter.type";
import AppHeader from "./components/header/AppHeader.vue";
import AppFilters from "./components/filters/AppFilters.vue";
import AppList from "./components/list/AppList.vue";
import AppAddTask from "./components/addTask/AppAddTask.vue";
import AppFooter from "./components/footer/AppFooter.vue";

interface State {
  todos: Todo[];
  activeFilter: Filter;
}

export default {
  components: {
    AppHeader,
    AppFilters,
    AppList,
    AppAddTask,
    AppFooter,
  },
  data(): State {
    return {
      todos: [],
      activeFilter: "All",
    };
  },
  computed: {
    filterdTodos(): Todo[] {
      switch (this.activeFilter) {
        case "Active":
          return this.activeTodos;
        case "Done":
          return this.doneTodos;
        case "All":
        default:
          return this.todos;
      }
    },
    stats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length,
      };
    },
    activeTodos(): Todo[] {
      return this.todos.filter((todo) => !todo.completed);
    },
    doneTodos(): Todo[] {
      return this.todos.filter((todo) => todo.completed);
    },
  },
  methods: {
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo) => todo.id === id);

      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed;
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    addTodo(todo: Todo) {
      this.todos.push(todo);
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter;
    },
  },
};
</script>

<template>
  <div class="app">
    <header class="app-header">
      <AppHeader />
    </header>
    <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />
    <main class="app-main">
      <AppList
        :todos="filterdTodos"
        @toggleTodo="toggleTodo"
        @removeTodo="removeTodo"
      />
      <AppAddTask :todo="todos" @add-todo="addTodo" />
    </main>

    <footer class="app-footer">
      <AppFooter :stats="stats" />
    </footer>
  </div>
</template>

<style scoped lang="scss">
@import "./assets/main.scss";

.title1 {
  color: $green;
  font-size: 30px;
  font-weight: 500;
}

.app {
  height: 100vh;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
</style>
