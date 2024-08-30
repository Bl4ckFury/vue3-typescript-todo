<script lang="ts">
import type { PropType } from "vue";
import DeleteIcon from "../icons/DeleteIcon.vue";
import CheckIcon from "../icons/CheckIcon.vue";
import Todo from "../../models/Item.interface";

export default {
  components: {
    CheckIcon,
    DeleteIcon,
  },
  props: {
    todo: {
      type: Object as PropType<Todo>,
      required: true,
    },
  },
  methods: {
    toggleTodo() {
      this.$emit("toggleTodo", this.todo.id);
    },
    removeTodo() {
      this.$emit("removeTodo", this.todo.id);
    },
  },
  emits: {
    toggleTodo: (id: number) => Number.isInteger(id),
    removeTodo: (id: number) => Number.isInteger(id),
  },
};
</script>

<template>
  <li
    class="todo-item"
    :class="{ 'todo-item__done': todo.completed }"
    @click="toggleTodo"
  >
    <div v-if="todo.completed" class="todo-item__status">
      <CheckIcon class="check-icon" />
    </div>
    <span class="todo-item__text">{{ todo.text }}</span>
    <button class="todo-item__btn" @click.stop="removeTodo">
      <DeleteIcon class="delete-icon" />
    </button>
  </li>
</template>

<style lang="scss">
@import "../../assets/main.scss";

.todo-item {
  margin: 10px;
  display: flex;
  cursor: pointer;
  align-items: center;
  -webkit-line-clamp: 1;
  justify-content: space-between;
  border-bottom: 1px solid $green;
  .todo-item__text {
    font-size: 14px;
    text-transform: uppercase;
  }
  .todo-item__btn {
    cursor: pointer;
  }
  .check-icon {
    color: $green;
  }
  .delete-icon {
    &:hover {
      color: $green;
    }
  }
}

.todo-item__done {
  text-decoration: line-through;
}
</style>
