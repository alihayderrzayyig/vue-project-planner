<template>
  <!-- <h1>Edit Project Page</h1> -->
  <!-- <p>project id : {{ id }}</p> -->

  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" required v-model="title" />
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Edit Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      //   id: this.$route.params.id,
      uri: "http://localhost:3000/projects/" + this.id,
      title: "",
      details: "",
      complete: "",
    };
  },
  methods: {
    handleSubmit() {
      var x = {
        title: this.title,
        details: this.details,
        complete: this.complete,
      };

      fetch(this.uri, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(x),
      })
        .then(() => {
          //   this.$emit("complete", this.project.id);
          this.$router.push({ name: "home" });
        })
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    fetch(this.uri, {
      method: "GET",
    })
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
        this.complete = data.complete;
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style>
</style>