<template>
  <div class="projects-container">
    <b-button class="float-right" size="lg" variant="success">+ NEW PROJECT</b-button>
    <div class="shadow table">
      <b-table hover :items="projects" :fields="fields">
        <template #cell(project)="data">
          <span class="margin-right-md">{{ data.item.project_name }}</span>
          <b-button size="sm" class="margin-right-md" variant="primary" @click="updateProject(data.item.project_id)">EDIT</b-button>
          <b-button size="sm" class="margin-right-md" variant="danger" @click="deleteProject(data.item.project_id)">DELETE</b-button>
        </template>
        <template #cell(customer)="data">
          <span class="margin-right-md">{{ data.item.customer_name }}</span>
          <b-button size="sm" class="margin-right-md" variant="primary" @click="updateCustomer(data.item.customer_id)">EDIT</b-button>
        </template>
        <!-- <template #cell(actions)="data">
          <b-button size="sm" @click="updateCustomer(data.item.customer_id)">GO TO TASKS</b-button>
        </template> -->
      </b-table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Project",
  data () {
    return {
      projects: [],
      fields: ['project_id', 'project', 'customer']
    }
  },
  methods: {
    getAllProjects: function() {
      this.$http.get('http://localhost:8000/project/')
      .then(projects => {
          console.log('projects: ' + JSON.stringify(projects))
          this.projects = projects.data;
      })
      .catch(error => console.log(error))
      },
      updateProject: function(project) {
        console.log('Updated project: ' + project);
      },
      updateCustomer: function(customer) {
        console.log('Updated customer: ' + customer);
      },
      deleteProject: function(project) {
        console.log('Deleted customer: ' + project);
      }
  },
  mounted() {
    this.getAllProjects();
  }
};
</script>

<style scoped>

</style>
