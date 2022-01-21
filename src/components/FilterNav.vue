<template>
  <nav class="filter-nav">
    <button :class="{ active: current === 'all' }" @click="updateFilter('all')">
      View All
    </button>
    <button
      :class="{ active: current === 'completed' }"
      @click="updateFilter('completed')"
    >
      Completed
    </button>
    <button
      :class="{ active: current === 'ongoing' }"
      @click="updateFilter('ongoing')"
    >
      Ongoing
    </button>
  </nav>
</template>

<script>
export default {
  props: ["current"],
  methods: {
    updateFilter(filter) {
      this.$emit("updateFilter", filter);
    },
  },
};
</script>

<style>
.filter-nav button {
  background: none;
  border: none;
  color: #bbb;
  outline: none;
  font-size: 12px;
  text-transform: uppercase;
  margin-right: 10px;
  font-weight: bold;
  cursor: pointer;
}

.filter-nav button.active {
  color: #00ce89;
}
</style>
