<template>
  <h1>Edit Project</h1>
  <form @submit.prevent="handleSubmit">
    <label>Title</label>
    <input type="text" v-model="title" required />
    <label>Details</label>
    <textarea v-model="details" required></textarea>
    <button type="submit">Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/projects/" + this.id,
    };
  },
  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          title: this.title,
          details: this.details,
        }),
      })
        .then((response) => {
          this.$router.push("/");
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
  mounted() {
    fetch(this.uri).then((response) => {
      response.json().then((data) => {
        this.title = data.title;
        this.details = data.details;
      });
    });
  },
};
</script>

<style></style>
