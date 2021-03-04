<template>
  <div class="projects-container">
    <b-button
      class="float-right"
      size="lg"
      variant="success"
      @click="openForm({}, 'new')"
      >+ NEW PROJECT</b-button
    >
    <div class="shadow table">
      <b-table hover :items="projects" :fields="fields">
        <template #cell(project)="data">
          <span class="margin-right-md">{{ data.item.project_name }}</span>
          <b-button
            size="sm"
            class="margin-right-md"
            variant="primary"
            @click="openForm(data.item, 'update')"
            >EDIT</b-button
          >
          <b-button
            size="sm"
            class="margin-right-md"
            variant="danger"
            @click="deleteProject(data.item)"
            >DELETE</b-button
          >
        </template>
        <template #cell(customer)="data">
          <span class="margin-right-md">{{ data.item.customer_name }}</span>
          <b-button
            size="sm"
            class="margin-right-md"
            variant="primary"
            @click="openForm(data.item, 'update')"
            >EDIT</b-button
          >
        </template>
        <!-- <template #cell(actions)="data">
          <b-button size="sm" @click="updateCustomer(data.item.customer_id)">GO TO TASKS</b-button>
        </template> -->
      </b-table>
    </div>
    <div class="form-wrapper">
      <div class="form-container shadow" v-if="isHidden">
        <label for="name">Project Id</label><br />
        <input :disabled="isDisabled" v-model="updatedProjectId" />
        <br />
        <label for="name">Customer Id</label><br />
        <input v-model="updatedCustomerId" />
        <br />
        <label for="name">Project Name</label><br />
        <input v-model="updatedProjectName" /><br />
        <b-button class="main-button" type="button" @click="handleEvents"
          >UPDATE PROJECT</b-button
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Project",
  data() {
    return {
      projects: [],
      fields: ["project_id", "project", "customer"],
      updatedProjectId: "",
      updatedCustomerId: "",
      updatedProjectName: "",
      isHidden: false,
      eventTypes: "",
      isDisabled: false
    };
  },
  methods: {
    getAllProjects: function () {
      this.$http
        .get("http://localhost:8000/project/")
        .then((projects) => {
          console.log("projects: " + JSON.stringify(projects));
          this.projects = projects.data;
        })
        .catch((error) => console.log(error));
    },
    updateProject: function () {
      console.log( "Updated customer: " + JSON.stringify(this.updatedProjectName));

      this.$http
        .put("http://localhost:8000/project/id/" + this.updatedProjectId, {
          id: this.updatedProjectId,
          customer_id: this.updatedCustomerId,
          name: this.updatedProjectName,
        })
        .then((updatedProject) => {
          console.log("updatedProject: " + JSON.stringify(updatedProject));
          this.projects.forEach((project) => {
            if (project.project_id === this.updatedProjectId) {
              project.project_name = this.updatedProjectName;
              project.customer_id = this.updatedCustomerId;
            }
          });
        })
        .catch((error) => console.log(error));
    },
    deleteProject: function (project) {
      console.log("Deleted project: " + JSON.stringify(project));
      this.$http
        .delete("http://localhost:8000/project/id/" + project.project_id)
        .then((deletedProject) => {
          console.log(deletedProject);
          let index = this.projects.findIndex((p) => {
            return p.project_id === project.project_id;
          });
          console.log("index: " + index);
          this.$delete(this.projects, index);
        })
        .catch((error) => console.log(error));
    },
    addNewProject: function () {
      this.$http
        .post("http://localhost:8000/project/", {
          customer_id: this.updatedCustomerId,
          name: this.updatedProjectName,
        })
        .then((project) => {
          let newProject = project.data[0];
          newProject["actions"] = {};

          console.log("addNewProject: " + JSON.stringify(newProject));
          this.projects.push(newProject);
        })
        .catch((error) => console.log(error));
    },
    openForm: function (project, type) {
      console.log('open form click')
      this.isHidden = true;
      this.isDisabled = false;
      this.eventTypes = type;

      if (Object.keys(project).length > 0) {
        console.log('project: ' + JSON.stringify(project))
        this.updatedProjectId = project.project_id;
        this.updatedCustomerId = project.customer_id;
        this.updatedProjectName = project.project_name;
      } else {
        this.resetForm();
        this.isDisabled = true;
      }

    },
    handleEvents: function () {
      if (this.eventTypes === "new") {
        this.addNewProject();
      } else {
        this.updateProject();
      }
    },
    resetForm: function () {
      console.log('reset form')
      this.updatedProjectId = '';
      this.updatedCustomerId = '';
      this.updatedProjectName = '';
    }
  },
  mounted() {
    this.getAllProjects();
  },
};
</script>

<style scoped>
.form-container {
  margin-top: 40px;
  width: 30%;
  height: 225px;
  background: #7593ff;
  border-radius: 10px;
  border-color: #7593ff;
  color: white;
  margin: auto;
  padding: 15px;
}
</style>
