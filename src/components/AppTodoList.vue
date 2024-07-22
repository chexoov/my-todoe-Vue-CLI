<template>
  <ul class="todo-list">
    <!-- создаем массив задач v-for -->
    <AppTodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @toggle-todo="toggleTodo"
      @remove-todo="removeTodo"
    />
  </ul>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import AppTodoItem from "./AppTodoItem.vue";
import { Todo } from "../types/Todo";



export default defineComponent({
  components: {
    AppTodoItem,
  },
  // принимаем значеиние от родительского компонента
  props: {
    todos: {
      // Здесь объявляется, что prop должен быть массивом с определёнными свойствами.
      type: Array as PropType<Todo[]>,
      
  }
  },
  // ловим события и отправляем их в родительский компонент
  methods: {

    // метод перечеркивания 
    toggleTodo(id: number) {
      this.$emit("toggleTodo", id);
    },
    // метод удаления
    removeTodo(id: number) {
      this.$emit("removeTodo", id);
    },
  },
  emits: {
    toggleTodo: (id: number) => Number.isInteger(id),
    removeTodo: (id: number) => Number.isInteger(id),
  },
});
</script>
