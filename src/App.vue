<template>
  <div id="app">
    <AppHeader />
    <!-- ячейка задач  -->
    <AppFilters :activeFilter="activeFilter" @set-filter="setFilter"/>
   
    <!-- карточка с задачами -->
    <main class="app-main">
      <!-- :todos="filterdTodos" - закидываем отфильтрованные компоненты  -->
      <AppTodoList
        :todos="filterdTodos"
        @toggle-todo="toggleTodo"
        @remove-todo="removeTodo"
      />

      <!-- ставим прослушиваине события  -->
      <!-- @add-todo="addTodo" - передаем в компонент AppAddTodo метод addTodo - обькт задач, которые добаявтся в список дел  -->
      <AppAddTodo @add-todo="addTodo" />
    </main>

    <AppFooter :stats="stats"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AppHeader from "./components/AppHeader.vue";
import AppFilters from "./components/AppFilters.vue";
import AppTodoList from "./components/AppTodoList.vue";
import AppAddTodo from "./components/AppAddTodo.vue";
import AppFooter, { Stats } from "./components/AppFooter.vue";
import { Todo } from "./types/Todo";
import { Filter } from "./types/Filter";


// State (просто название) будет состоять из элемента todos, внутри котрого будут наши данные
// передаем обязательные значения
interface State {
  // в конце ставим массив = [] -  озночает,  что массив может быть пустым или содержать любое количество элементов указанного типа.
  todos: Todo[],
  activeFilter: Filter,
}

export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppTodoList,
    AppAddTodo,
    AppFooter,
  },
  // data - функция которая возвращает объект, определнного типо, блягодаря типу State
  // : State - тип возвращаемого значения
  data(): State {
    return {
      // название свойства (он массив начинаем с [] )
      todos: [
        { id: 0, text: "Текст задачи 1", completed: true },
        { id: 1, text: "Текст задачи 2", completed: false },
        { id: 2, text: "Текст задачи 3", completed: false },
      ],
      activeFilter: "All",
    };
  },
  // фильтруем задачи, чтобы они были в своей ячейке стостояния задачи 
  computed: {
    //  вычисляем активные задачи
    activeTodos(): Todo[] {
      return this.todos.filter((todo: Todo) => !todo.completed);
    },
    // вычисляем выполенные задачи 
    doneTodos (): Todo[] {
      return this.todos.filter((todo: Todo) => todo.completed)
    },
    // возвращаем массив Todo[]
    filterdTodos(): Todo[] {
      switch (this.activeFilter) {
        // если выбрал All - возвращаем все задачи
        
        // если выбрал Active - возвращаем задачи которые не выполнены, по флагу !completed у которых folse
        case 'Active':
          return this.activeTodos
        case 'Done':
        // если выбрал Done - возвращаем задачи которые выполнены true
          return this.doneTodos
          //// если выбрал All - возвращаем все задачи - по умолчанию
        case 'All':
        default:
          return this.todos
      }
    },
    // описиывает футер, количество выполненных и не выполненных задач 
    stats(): Stats {
      // активные задачи 
      return {
        // возвращаем длину  задач Active
        active: this.activeTodos.length,
        // возвращаем длину задач Done
        done: this.doneTodos.length
      }
    },
  },
  
  methods: {
    // передаем созданные задачи в список  AppTodoList
    addTodo(todo: Todo) {
      this.todos.push(todo);
    },
    // метод перечеркивания
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id);
      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed;
      }
    },
    // метод удаления
    removeTodo(id: number) {
      // filter - убираем те элементы значенмя айди которых не равны id
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id);
    },
    // setFilter (функция) которая получает на входе filter типа Filter
    setFilter(filter: Filter) {
      // записываем получаемое значение фильтра в свойство activeFilter
      this.activeFilter = filter;
    },
  },
});
</script>
