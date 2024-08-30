<script lang="ts">
import type { PropType } from "vue";
import Filter from "../../models/Filter.type";

interface State {
  filters: Filter[];
}

export default {
  props: {
    activeFilter: {
      type: String as PropType<Filter>,
      riquired: true,
    },
  },
  data(): State {
    return {
      filters: ["All", "Active", "Done"],
    };
  },
  methods: {
    setFilter(filter: Filter) {
      this.$emit("setFilter", filter);
    },
  },
  emits: {
    setFilter: (filter: Filter) => filter,
  },
};
</script>

<template>
  <aside class="app-filters">
    <section class="toggle-group">
      <button
        class="button"
        v-for="filter in filters"
        :key="filter"
        :class="{ 'button--primary': activeFilter === filter }"
        @click="setFilter(filter)"
      >
        {{ filter }}
      </button>
    </section>
  </aside>
</template>

<style scoped lang="scss">
@import "../../assets/main.scss";

.toggle-group {
  margin-bottom: 10px;
  .button {
    cursor: pointer;
    font-size: 22px;
    margin: 5px 10px;
    font-weight: 500;
    padding: 5px 15px;
    border-radius: 20px;
    text-transform: uppercase;
    transition: all 0.2s ease-in-out;
  }
}

.button--primary {
  color: $green;
  border: 1px solid $green;
}
</style>
