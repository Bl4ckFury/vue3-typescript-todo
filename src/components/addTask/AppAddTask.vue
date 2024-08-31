<script lang="ts">
import CloseIcon from "../icons/CloseIcon.vue";
import PlusTaskIcon from "../icons/PlusTaskIcon.vue";
import Todo from "../../models/Item.interface";
import type { PropType } from "vue";

interface State {
  error: boolean;
  inputText: string;
  isFormVisible: boolean;
}

export default {
  components: {
    CloseIcon,
    PlusTaskIcon,
  },
  props: {
    todo: {
      type: Object as PropType<Todo>,
      riquired: true,
    },
  },
  data(): State {
    return {
      error: "",
      inputText: "",
      isFormVisible: false,
    };
  },
  mounted() {
    try {
      const todos = JSON.parse(localStorage.getItem('todos'))

      this.$emit("initTodos", todos);
    } catch(e) {
      this.todo = []
    }
  },
  methods: {
    showForm() {
      this.isFormVisible = true;
    },
    closeForm() {
      (this.error = ""), (this.isFormVisible = false);
      this.inputText = "";
    },
    addTodoItem() {
      if (this.inputText === "" || this.inputText.length <= 3) {
        return this.error = "Заполните поле";
      }

      this.$emit("addTodo", {
        id: Date.now(),
        text: this.inputText,
        completed: false,
      });
      this.inputText = "";
      this.error = "";
    },
  }
};
</script>

<template>
  <section class="add-todo">
    <span class="add-todo__error">{{ error }}</span>
    <form v-if="isFormVisible" class="add-todo__form" @submit.prevent="addTodo">
      <div class="text-input text-input--focus">
        <input v-model="inputText" class="input" />
        <button class="close-button" @click="closeForm">
          <CloseIcon />
        </button>
        <button @click="addTodoItem" class="button button--filled">
          Добавить задачу
        </button>
      </div>
    </form>
    <button v-else class="add-todo__show" @click="showForm">
      <PlusTaskIcon />
    </button>
  </section>
</template>

<style scoped lang="scss">
@import "../../assets/main.scss";

.add-todo {
  display: flex;
  align-items: center;
  flex-direction: column;
  .add-todo__error {
    color: $pink;
    font-size: 15px;
    font-weight: 600;
    text-transform: uppercase;
  }
  .text-input {
    display: flex;
    align-items: center;
    flex-direction: column;
    .input {
      height: 40px;
      width: 300px;
      padding: 5px;
      color: $white;
      border-radius: 5px;
      margin-bottom: 10px;
      border: 1px solid $green;
      background-color: transparent;
      transition: all 0.2s ease-in-out;
      &:focus {
        box-shadow: 0px 0px 12px 2px $green;
        -moz-box-shadow: 0px 0px 12px 2px $green;
        -webkit-box-shadow: 0px 0px 12px 2px $green;
        outline: none;
      }
    }
    .button {
      width: 180px;
      color: $green;
      padding: 12px;
      margin: 10px 15px;
      font-size: 16px;
      cursor: pointer;
      font-weight: 700;
      border-radius: 5px;
      background-color: $white;
      transition: all 0.1s ease-in-out;
      &:hover {
        transform: scale(1.07);
      }
    }
  }
  .add-todo__show {
    width: 300px;
    padding: 5px;
    color: $night;
    cursor: pointer;
    padding-top: 10px;
    margin-bottom: 30px;
    border-radius: 14px;
    border: 1px solid $green;
    background-color: $green;
    transition: all 0.2s ease-in-out;
    &:hover {
      color: $green;
      background-color: transparent;
    }
  }
  .close-button {
    cursor: pointer;
  }
}
</style>
