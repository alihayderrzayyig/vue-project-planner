<template>
  <div class="home">
    <div v-if="projects.length">
      <!-- {{ projects }} -->
      <div v-for="project in projects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handelDelete"
          @complete="toggleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
export default {
  name: "HomeView",
  components: { SingleProject },

  data() {
    return {
      projects: [],
    };
  },

  methods: {
    handelDelete(id) {
      // console.log(id);
      this.projects = this.projects.filter((data) => {
        return id !== data.id;
      });
    },
    toggleComplete(id) {
      let project = this.projects.find((project) => {
        return project.id === id;
      });
      project.complete = !project.complete
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((data) => data.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err));
  },
};
</script>
 