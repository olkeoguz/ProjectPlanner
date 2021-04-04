<template>
  <h1>Edit project</h1>
  <form @submit.prevent="handleSubmit">
    <label>Title</label>
    <input type="text" required v-model="title" />
    <label>Details </label>
    <textarea required v-model="details"></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/projects/",
    };
  },
  async mounted() {
    try {
      const res = await fetch(this.uri + this.id);
      const data = await res.json();
      this.title = data.title;
      this.details = data.details;
    } catch (error) {
      console.log(error.message);
    }
  },
  methods: {
    async handleSubmit() {
      const project = {
        title: this.title,
        details: this.details,
      };
      try {
        await fetch(this.uri + this.id, {
          method: "PATCH",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(project),
        });
        this.$router.push("/");
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style>
h1 {
  text-align: center;
}
</style>