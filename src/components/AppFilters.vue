<template>
  <aside class="app-filters">
    <section class="toggle-group">
      <!-- 1й filter - это наши значения  all, active, done -->
       <!-- отрисовываем каждую кнопку в цикле filter они прописаны в filters.ts -->
        <!-- @click="setFilter(filter)" - прослушиваем по какой кнопке произошел клик  -->
      <button
        v-for="filter in filters"
        :key="filter"
        @click="setFilter(filter)"
        class="button"
        :class="{ 'button--primary': activeFilter === filter }"
      >
        {{ filter }}
      </button>
      
    </section>
  </aside>
</template>

<script lang="ts">
import { PropType, defineComponent } from "vue";
import { Filter } from "@/types/Filter";

interface State {
  // ограчиваем положение фильров, так как в filters.ts есть только 3 значения ['All', 'Active', 'Done'], больше мы туда ничего вписать не сможем
  filters: Filter[];
}

export default defineComponent({
  props: {
    activeFilter: {
      type: String as PropType<Filter>,
      // строка может быть одним из этиз значений и больше никаким
      required: true,
    },
  },
  // отрисовываем фильтры в каждом цикле
  data(): State {
    return {
      filters: ["All", "Active", "Done"],
    };
  },
  // добовляем прослушиватель событий на кжадую кнопку 
  methods: { 
    // ожидаем  filter типа Filter 
    setFilter( filter: Filter) {
      // эмитим событие и отправляем filter , используем в родительском компоненте  <AppFilters :activeFilter="activeFilter" @set-filter="setFilter"/>
      this.$emit("setFilter", filter);
    }
  },
  emits: {
    // функиця , которая принимет на вход параметр filter типа Filter и просто смотрим на наличие => filter
    setFilter: (filter: Filter) => filter
  },
});
</script>
