<template>
  <div class="project">
    <Project msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import Project from "@/components/Project.vue";

export default {
  name: "Projects",
  components: {
    Project
  }
};
</script>
