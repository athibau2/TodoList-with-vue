<template>
  <div id='task-list'>
    <v-list-item-group>

      <v-list-item v-for="task in tasks" :key="task._id">

        <template>
          <v-list-item-action>

            <v-btn icon v-on:click="updateTask(task)">

              <v-icon v-if="task.Done === true">check_box</v-icon>
              <v-icon v-else>check_box_outline_blank</v-icon>

            </v-btn>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>

              {{task.Text}}

            </v-list-item-title>
            <v-list-item-subtitle>

              {{task.Date}}

            </v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>

            <v-btn icon v-on:click="deleteTask(task)">

              <v-icon color="red">delete</v-icon>
            </v-btn>
          </v-list-item-action>
        </template>
      </v-list-item>
    </v-list-item-group>
  </div>
</template>

<script>
export default {
  name: 'TaskList',
  props: {
    tasks: {
        Type: Array,
        default: () => []
    },
    updateTask: {
        Type: Function
    },
    deleteTask: {
        Type: Function
    }
  }
}
</script>