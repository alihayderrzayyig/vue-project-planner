<template>
  <div class="home">
    <FilterNav @filter="current = $event" :current="current" />
    <div v-if="filteredProject.length">
      <!-- {{ projects }} -->
      <div v-for="project in filteredProject" :key="project.id">
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
import FilterNav from "../components/FilterNav.vue";
export default {
  name: "HomeView",
  components: { SingleProject, FilterNav },

  data() {
    return {
      projects: [],
      current: "all",
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
      project.complete = !project.complete;
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((data) => data.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err));
  },
  computed: {
    filteredProject() {
      // if (this.current === "completed") {
      //   return this.projects.filter((res) => {
      //     return res.complete === true;
      //   });
      // } else if (this.current === "ongoing") {
      //   return this.projects.filter((res) => {
      //     return res.complete === false;
      //   });
      // } else {
      //   return this.projects.filter((res) => {
      //     return res;
      //   });
      // }
      if (this.current === "completed") {
        return this.projects.filter((res) => res.complete);
      }
      if (this.current === "ongoing") {
        return this.projects.filter((res) => !res.complete);
      }
      return this.projects;
    },
  },
};
</script>
 