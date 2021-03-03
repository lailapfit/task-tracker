<template>
    <div class="tasks">
        <Task />
    </div>
</template>

<script>
import Task from "@/components/Task.vue"

export default {
  name: "Tasks",
  components: {
      Task
  }
}
</script>