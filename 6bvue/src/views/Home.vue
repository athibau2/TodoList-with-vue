<template>
  <div class="home">
    <v-card class="mx-auto" max-width="900" v-if="fetched">

      <new-task-form :form="form" :createTask="createTask"></new-task-form>

      <v-divider></v-divider>
      <v-list subheader two-line flat>
        <v-subheader>

          {{user.UserName}}'s Tasks:

        </v-subheader>

        <task-list :tasks="tasks" :updateTask="updateTask" :deleteTask="deleteTask"></task-list>

      </v-list>
    </v-card>
  </div>
</template>

<script>
import { getCurrentDate, formatDate } from '@/util'
import TaskList from '@/components/TaskList.vue'
import NewTaskForm from '@/components/NewTaskForm.vue'

export default {
  name: 'Home',
  components: {
    TaskList,
    NewTaskForm
  },
  props: {
    user: {
        Type: Object,
        default: {
            Type: Object,
            UserName: ""
        }
    }
  },
  data: () => ({
    fetched: false, // This keeps us from getting an error when the page loads, but there's no data
    tasks: [], // This will hold the list of tasks you get from your API
    form: {
      Text: "",
      Date: getCurrentDate()
    },
  }),
  mounted() {
    this.readTasks()
  },
  methods: {
    createTask(form) {
      fetch(
        `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks`,
        {
          method: `POST`,
          credentials: `include`,
          headers: {"Content-Type": "application/json"},
          body: JSON.stringify({"Text": form.Text, "Date": form.Date})
        }
      ).then(response => {
        if (response.ok) {
          return response.json()
        }
      }).then(data => {
        this.form.Text = ""
        this.form.Date = formatDate(getCurrentDate())
        this.readTasks()
      })
    },
    readTasks() {
      fetch(
        `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks`,
        {
          credentials: `include`,
        }
      ).then(response => {
        if (response.ok) {
          return response.json()
        }
      }).then(data => {
        this.fetched = true
        this.tasks = data
      })
    },
    updateTask(task) {
      fetch(
        `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/${task._id}`,
        {
          method: `PUT`,
          credentials: `include`,
          headers: {"Content-Type": "application/json"},
          body: JSON.stringify({"Done": !task.Done})
        }
      ).then(response => {
        if (response.ok) {
          return response.json()
        }
      }).then(data => {
        this.readTasks()
      })
    },
    deleteTask(task) {
      fetch(
        `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/${task._id}`,
        {
          method: `DELETE`,
          credentials: `include`,
        }
      ).then(response => {
        if (response.ok) {
          this.readTasks()
        }
      })
    }
  }
}
</script>

<style scoped>
.form {
  padding: 0 1rem;
}
</style>