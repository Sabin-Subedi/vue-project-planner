<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="showDetail">{{ project.title }}</h3>
      <i @click="handleDelete" class="icon fas fa-trash"></i>
      <router-link :to="{ name: 'Edit', params: { id: project.id } }">
        <i class="icon fas fa-pencil"></i>
      </router-link>
      <i
        v-if="!project.complete"
        @click="completeProject"
        class="icon far fa-check"
      ></i>
      <i
        v-if="project.complete"
        @click="completeProject"
        class="icon far fa-times"
      ></i>
    </div>
    <div v-if="showDetails">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },

  methods: {
    showDetail() {
      this.showDetails = !this.showDetails;
    },
    handleDelete() {
      fetch(this.uri, {
        method: "DELETE",
      }).then((response) => {
        this.$emit("delete", this.project.id);
      });
    },
    completeProject() {
      fetch(this.uri, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          complete: !this.project.complete,
        }),
      }).then((response) => this.$emit("completeProject", this.project.id));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background-color: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
  flex: 1;
}
.actions {
  display: flex;
  align-items: center;
}
.icon {
  font-size: 20px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.icon:hover {
  color: #777;
}
.complete {
  border-left: 4px solid #00ce89;
}
</style>
