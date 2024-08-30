<script lang="ts">
import { PropType } from "vue";

export interface Stats {
  active: number;
  done: number;
}

export default {
  props: {
    stats: {
      type: Object as PropType<Stats>,
      required: true,
    },
  },
};
</script>

<template>
  <footer class="app-footer">
    {{ stats.active }} активных задач, {{ stats.done }} выполнено
  </footer>
</template>

<style scoped lang="scss">
@import "../../assets/main.scss";
</style>
