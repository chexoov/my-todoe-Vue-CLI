<template>
    <footer class="app-footer">{{ stats.active }} количество задач , {{ stats.done }} выполенные задачи </footer>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

// описываем свойства компонента stats
export interface Stats {
    // количество активных задач
    active: number,
    // количество выполненных задач
    done: number
}


export default defineComponent({
  props: {
      stats: {
          type: Object as PropType<Stats>,
          required: true
      }
  }
});
</script>