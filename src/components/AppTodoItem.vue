<template>
  <li
    class="todo-item"
    :class="{ 'todo-item--done': todo.completed }"
    @click="toggleTodo"
  >
    <div class="todo-item__status">
      <i class="bi bi-check2"></i>
    </div>
    <span class="todo-item__text">{{ todo.text }}</span>
    <!-- @click.stop - не вызовет срабатывание события - @click="togglerTodo" -->
    <button class="todo-item__remove-button" @click.stop="removeTodo">
      <i class="bi bi-trash3"></i>
    </button>
  </li>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
// импортируем ts из папки types, где описаны все обязательные свойства
import { Todo } from "../types/Todo";

export default defineComponent({
  props: {
    // название пропса
    todo: {
      // Здесь объявляется, что prop должен быть объектом с определёнными свойствами.
      type: Object as PropType<Todo>,
      // делаем его обязательным
      required: true,
    },
  },
  // создаем метод
  methods: {
    // Функция this.$emit() используется во Vue для генерации пользовательских событий, которые могут быть обработаны родительским компонентом или другими компонентами в приложении. В данном случае событие 'toggleTodo' генерируется с параметром this.todo.id,
    toggleTodo() {
      this.$emit("toggleTodo", this.todo.id);
    },
    removeTodo() {
      this.$emit("removeTodo", this.todo.id);
    },
  },
  // типизация emit
  // наш ключ, это название функции toggleTodo и ее значение ....
  // для того чтобы мы не смогли поменять значение с this.todo.id на другое -  допустим  на this.todo.compoleted
  emits: {
    toggleTodo: (id: number) => Number.isInteger(id),
    removeTodo: (id: number) => Number.isInteger(id),
  },
});
</script>
