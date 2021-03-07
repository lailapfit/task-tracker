<template>
  <div class="tasks-container">
    <b-button
      class="float-right"
      size="lg"
      variant="success"
      @click="openForm({}, 'newTask')"
      >+ NEW TASK</b-button
    >
    <div class="shadow">
      <b-table hover :items="taskLogs" :fields="fields">
        <template #cell(actions)="data">
          <b-button
            size="sm"
            class="margin-right-md"
            variant="primary"
            @click="openForm(data.item, 'updateTaskLog')"
            >EDIT</b-button
          >
          <b-button
            size="sm"
            class="margin-right-md"
            variant="success"
            @click="openForm(data.item, 'newTaskLog')"
            >+ NEW TASK LOG</b-button
          >
          <b-button
            size="sm"
            class="margin-right-md"
            variant="danger"
            @click="deleteTaskLog(data.item)"
            >DELETE</b-button
          >
        </template>
      </b-table>
    </div>
    <div class="form-wrapper">

      <!-- task log -->
      <div class="form-wrapper">
        <div class="form-container shadow" v-if="isHidden">
          <label for="taskLogId">Task Log Id</label><br />
          <input v-model="taskLogId" />
          <br />

          <label for="taskId">Task Id</label><br />
          <input v-model="taskId" />
          <br />

          <label for="userId">User Id</label><br />
          <input v-model="userId" />
          <br />

          <label for="description">Task Description</label><br />
          <input v-model="description" />
          <br />

          <label for="startTime">Start Time</label><br />
          <input v-model="startTime" /><br />

          <label for="endTime">End Time</label><br />
          <input v-model="endTime" /><br />

          <label for="durationMinutes">Duration Minutes</label><br />
          <input v-model="durationMinutes" /><br />

          <b-button class="main-button" type="button" @click="handleEvents"
            >SUBMIT TASK LOG</b-button
          >
        </div>
      </div>

      <div id="timer-container">
        <div class="values">00:00:00</div>
        <div>
          <button class="start-button" @click="startTask">Start</button>
          <button class="pause-button" @click="endTask">Pause</button>
          <button class="stop-button" @click="endTask">Stop</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Timer from "easytimer.js";
import * as moment from "moment/moment";
import $ from "jquery";
const timer = new Timer();

timer.addEventListener("secondsUpdated", function() {
  $("#timer-container .values").html(timer.getTimeValues().toString());
});

timer.addEventListener("started", function() {
  $("#timer-container .values").html(timer.getTimeValues().toString());
});

export default {
  name: "Task",
  data() {
    return {
      taskLogs: [],
      fields: [
        "task_id",
        "task_description",
        "user_email",
        "project_name",
        "duration_minutes",
        "actions",
      ],
      taskId: "",
      description: "",
      projectId: 1,
      taskLogId: "",
      userId: "",
      durationMinutes: "",
      projectName: "",
      startTime: "",
      endTime: "",
      startTimeDuration: "",
      endTimeDuration: "",
      isDisabledUserId: false,
      isDisabled: false,
      isHidden: false,
      eventTypes: "",

    };
  },
  methods: {
    getAllTaskLogs: function() {
      this.$http
        .get("http://localhost:8000/task-log/")
        .then((taskLogs) => {
          this.taskLogs = taskLogs.data;
        })
        .catch((error) => console.log(error));
    },
    addTaskLog: function() {
      console.log("Add new task log " + this.taskId);
      this.$http
        .post("http://localhost:8000/task-log/", {
          task_id: this.taskId,
          user_id: this.$userId,
          start_time: this.startTime
        })
        .then((taskLog) => {
          console.log("task log new: " + JSON.stringify(taskLog));
          let newTaskLog = taskLog.data[0];
          newTaskLog["actions"] = {};
          taskLog.duration_minutes = newTaskLog["duration_minutes"];
          this.projectId = newTaskLog["project_id"];
          this.project_name = newTaskLog["project_name"];
          this.description = newTaskLog["task_description"];
          this.taskId = newTaskLog["task_id"];
          this.$email = newTaskLog["user_email"];
          this.$userId = newTaskLog["user_id"];
           this.startTime = newTaskLog["start_time"];
          this.endTime = newTaskLog["end_time"];
        this.taskLogId = newTaskLog["id"];

          console.log("addNewTaskLog: " + JSON.stringify(newTaskLog));
          this.taskLogs.push(newTaskLog);
        })
        .catch((error) => console.log(error));
    },
    addNewTask: function() {
      this.projectId = 1;
      this.$http
        .post("http://localhost:8000/task/", {
          project_id: this.projectId,
          description: this.description,
        })
        .then((task) => {
          console.log("task new: " + JSON.stringify(task));
          let newTask = task.data[0];
          newTask["actions"] = {};
          newTask["duration_minutes"] = 0;
          newTask["project_id"] = this.projectId;
          newTask["project_name"] = this.project_name;
          newTask["task_description"] = newTask.description;
          newTask["task_id"] = newTask.id;
          newTask["user_email"] = this.$email;
          newTask["user_id"] = this.$userId;

          console.log("addNewTask: " + JSON.stringify(newTask));
          this.taskLogs.push(newTask);
        })
        .catch((error) => console.log(error));
    },
    startTask: function() {
      this.startTime = moment().format("HH:mm:ss");
      timer.start();
      console.log("start time: " + this.startTime);

      this.startTimeDuration = timer.getTimeValues().toString();
      console.log("start time duration: " + this.startTimeDuration);
      this.addTaskLog();
    },
    endTask: function() {
      this.endTime = moment().format("HH:mm:ss");
      console.log("end time: " + this.endTime);

      this.endTimeDuration = timer.getTimeValues().seconds;
      this.durationMinutes = this.endTimeDuration;
      timer.stop();
      console.log("end time duration: " + this.durationMinutes);
      this.updateTaskLog();
    },
    updateTask: function() {
      console.log("update task: " + this.taskId);

      this.$http
        .put("http://localhost:8000/task/id/" + this.taskId, {
          id: this.taskId,
          description: this.description,
          project_id: this.projectId,
        })
        .then((updatedTask) => {
          console.log("updatedTask: " + JSON.stringify(updatedTask));
          this.taskLogs.forEach((taskLog) => {
            if (taskLog.task_id === this.taskId) {
              taskLog.task_description = this.description;
              taskLog.project_id = this.projectId;
            }
          });
        })
        .catch((error) => console.log(error));
    },
    updateTaskLog: function() {
      console.log("update task log: " + this.taskLogId);

      this.$http
        .put("http://localhost:8000/task-log/id/" + this.taskLogId, {
          id: this.taskLogId,
          task_id: this.taskId,
          user_id: this.$userId,
          duration_minutes: this.durationMinutes,
          end_time: this.endTime
        })
        .then((updatedTaskLog) => {
          console.log("updatedTaskLog: " + JSON.stringify(updatedTaskLog));
          this.taskLogs.forEach((taskLog) => {
            if (taskLog.task_log_id === this.taskLogId) {
              taskLog.task_id = this.taskId;
              taskLog.user_id = this.$userId;
              taskLog.duration_minutes = this.durationMinutes;
            }
          });
          this.$router.go();
        })
        .catch((error) => console.log(error));
    },
    deleteTask: function(task) {
      console.log("delete task: " + task);
      this.$http
        .delete("http://localhost:8000/task/id/" + this.taskId)
        .then((deletedTask) => {
          console.log(deletedTask);
          let index = this.taskLogs.findIndex((t) => {
            return t.task_id === task.task_id;
          });
          console.log("index: " + index);
          this.$delete(this.taskLogs, index);
        })
        .catch((error) => console.log(error));
    },
    deleteTaskLog: function(taskLog) {
      console.log("delete task log: " + JSON.stringify(taskLog));
      this.$http
        .delete("http://localhost:8000/task-log/id/" + taskLog.task_log_id)
        .then((deletedTaskLog) => {
          console.log(deletedTaskLog);
          let index = this.taskLogs.findIndex((t) => {
            return t.task_log_id === taskLog.task_log_id;
          });
          console.log("index: " + index);
          this.$delete(this.taskLogs, index);
        })
        .catch((error) => console.log(error));
    },
    openForm: function(task, type) {
      console.log("open click");
      this.isHidden = true;
      this.resetForm();
    //   this.isDisabledUserId = false;
    //   this.isDisabledTaskLog = false;
      this.isDisabled = false;
      this.eventTypes = type;

      if (Object.keys(task).length > 0) {
        console.log("task: " + JSON.stringify(task));
        this.taskId = task.task_id;
        this.description = task.task_description;
        this.projectId = task.project_id;
        this.projectName = task.project_name;
        if (type !== "newTaskLog") {
          this.taskLogId = task.task_log_id;
          this.durationMinutes = task.duration_minutes;
        } else {
          this.isDisabledUserId = true;
        //   this.isDisabledTaskLog = true;
        }
      } else {
        this.isDisabled = true;
      }
    },
    handleEvents: function() {
      if (this.eventTypes === "newTask") {
        this.addNewTask();
      } else if (this.eventTypes === "newTaskLog") {
        this.addTaskLog();
      } else if (this.eventTypes === "updateTaskLog") {
        this.updateTaskLog();
      } else {
        this.updateTask();
      }
    },
    resetForm: function() {
      console.log("reset");
      this.taskId = "";
      this.description = "";
      this.projectId = "";
      this.taskLogId = "";
      this.durationMinutes = "";
      this.projectName = "";
    },
  },
  mounted() {
    this.userId = this.$userId;
    this.getAllTaskLogs();
  },
  watch: {
    userId: function(val) {
      this.$userId = val;
    },
  },
};
</script>

<style scoped>
.form-wrapper {
    display: flex;
    margin-top: 40px;
    justify-content: space-evenly;
}
.form-container {
    width: 450px;
    height: 100%;
    background: #7593FF;
    border-radius: 10px;
    border-color: #7593FF;
    color: white;
    margin: auto;
    padding: 15px;
    margin-right: 20px;
}

</style>
