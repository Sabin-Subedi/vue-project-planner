<template>
  <div class="home">
    <FilterNav @updateFilter="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProject" :key="project.id">
        <Card
          :project="project"
          @delete="handleDelete"
          @completeProject="handleCompleteProject"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Card from "../components/Card.vue";
import FilterNav from "../components/FilterNav.vue";

export default {
  name: "Home",
  components: { Card, FilterNav },

  data() {
    return { projects: [], current: "all" };
  },

  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    handleCompleteProject(id) {
      console.log("complete");
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },
  },

  computed: {
    filteredProject() {
      if (this.current === "completed") {
        return this.projects.filter((project) => project.complete);
      } else if (this.current === "ongoing") {
        return this.projects.filter((project) => !project.complete);
      } else {
        return this.projects;
      }
    },
  },

  mounted() {
    fetch(" http://localhost:3000/projects")
      .then((response) => response.json())
      .then((json) => {
        this.projects = json;
      })
      .catch((error) => {
        console.error(error);
      });
  },
};
</script>
