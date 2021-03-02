<template>
  <div class="projects-container">
    <ul v-for="project in projects" :key="project.project_id">
      <li>
        <div class="section shadow">
          <h3>Project: {{ project.project_name }}</h3>
          <h4>Customer: {{ project.customer_name }}</h4>
          <button class="secondary-button">UPDATE</button>
          <button class="secondary-button">DELETE</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Project",
  data () {
    return {
      projects: []
    }
  },
  methods: {
    getAllProjects: function() {
      this.$http.get('http://localhost:8000/project/')
      .then(projects => {
          console.log('projects: ' + JSON.stringify(projects))
          this.projects = projects.data;
      })
      .catch(error => console.log(error))}
  },
  mounted() {
    this.getAllProjects();
  }
};
</script>

<style scoped>

</style>
