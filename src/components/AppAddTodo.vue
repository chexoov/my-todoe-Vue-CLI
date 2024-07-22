<!-- добавить задачу -->
<template>
  <section class="add-todo">
    <!-- делаем отоброжение фона только в случае если isFormVisible = true  -->
     <!-- ставит прослушиватель событий - @submit , добавим .prevent - который предотвротит перезагрузку страницы  при отправке формы  -->
    <form v-if="isFormVisible" class="add-todo__form" @submit.prevent="addTodo">
      <button class="close-button" type="button" @click="closeForm">
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <!-- обновляем получаемые значения внутрь - todoText  -->
        <input v-model="todoText" class="input" />
      </div>
      <button class="button button--filled">Add task</button>
    </form>
    <!-- делаем отоброжение кнопки только в случае если isFormVisible = false -->
    <button v-else class="add-todo__show-form-button" @click="showForm">
      <i class="bi bi-plus-lg"></i>
    </button>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Todo } from "../types/Todo";

// определяем состояние isFormVisible
interface State {
  isFormVisible: boolean,
  todoText: string,
}

export default defineComponent({
  data(): State {
    return {
      // видимость таблицы добавить задачу
      isFormVisible: false,
      // текст задачи
      todoText: "",
    };
  },
  methods: {
    // отоброжение формы на плюс
    showForm() {
      this.isFormVisible = true;
    },
    //  выключаем отоброжение формы на крестик 
    closeForm() {
      this.isFormVisible = false;
    },
    // создаем обькт задач и передаем его в компонент AppTodoList
    addTodo() {
      // будет эмитить событие наверх в поле где видны все задачи 
      this.$emit("addTodo", {
        // генерируем id по текущей дате и времени 
        id: Date.now(),
        text: this.todoText,
        completed: false,
      })

      // очищаем поле ввода
      this.todoText = "";
    }
  },
  // типизация emit под наши значемния в Todo.ts
   emits: {
    addTodo: (todo: Todo) => todo
  } 
})
</script>
