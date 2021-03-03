<template>
    <div class="tasks-container">
        <div class="shadow">
            <b-button @click="addNewTask">+ NEW TASK</b-button>
            <b-table hover :items="taskLogs" :fields="fields">
                <template #cell(actions)="data">
                    <b-button size="sm" class="margin-right-md" variant="primary" @click="updateTask(data.item.task_log_id)">EDIT</b-button>
                    <b-button size="sm" class="margin-right-md" variant="success" @click="addTaskLog(data.item.task_id, data.item.user_id, data.item.project_id)">+ NEW TASK LOG</b-button>
                    <b-button size="sm" class="margin-right-md" variant="danger" @click="deleteTask(data.item.customer_id)">DELETE</b-button>
                </template>
            </b-table>
        </div>
    </div>
</template>

<script>
export default {
    name: "Task",
    data() {
        return {
            // taskLogsByTaskId: [],
            taskLogs: [],
            fields: ['task_id', 'task_description', 'user_email', 'project_name', 'duration_minutes', 'actions']
        }
    },
    methods: {
        getAllTaskLogs: function() {
            this.$http.get('http://localhost:8000/task-log/')
            .then(taskLogs => {
                this.taskLogs = taskLogs.data;
                // let sortedTaskLogs = this.sortTaskLogsByTaskId(this.taskLogs);
                // this.taskLogsByTaskId = this.objToArray(sortedTaskLogs);
            })
            .catch(error => console.log(error))
        },
        // sortTaskLogsByTaskId: function(taskLogs) {
        //     let taskLogsSorted = {};
        //     taskLogs.forEach(taskLog => {
        //         if (taskLogsSorted.hasOwnProperty(taskLog)) {
        //             taskLogsSorted[taskLog].push(taskLog);
        //         } else {
        //             taskLogsSorted[taskLog] = [taskLog];
        //         }
        //     })
        //     return taskLogSorted;
        // },
        // objToArray: function(obj) {
        //     return Object.keys(obj).map(o => {
        //         return { o: obj[o] };
        //     })
        // },
        addTaskLog: function(taskId, userId, projectId) {
            console.log('Add new task: ' + taskId + ' with ' + userId + ' in project ' + projectId);
        },
        addNewTask: function(projectId, userId) {
            console.log('Add new task in project ' + projectId + ' with user ' + userId);
        },
        updateTask: function(task) {
            console.log('update task: ' + task);
        },
        deleteTask: function(task) {
            console.log('delete task: ' + task);
        }
    },
    mounted() {
        this.getAllTaskLogs();
    }
}
</script>