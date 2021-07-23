<template>
  <v-app>
    <!-- Link to Material Icons-->
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    
    <app-bar v-bind:user="user"></app-bar>

    <v-content>
      <router-view :user="user"></router-view>
    </v-content>

  </v-app>
</template>

<script>
import AppBar from '@/components/AppBar.vue'
export default {
  name: 'App',
  components: {
    AppBar
  },
  asyncComputed: {
    user: {
      async get() {
        fetch(`${process.env.VUE_APP_API_ORIGIN}/api/v1/user`,
        {
          method: `GET`,
          credentials: `include`,
        }
      ).then(function(response) {
        if (response.ok) {
          console.log("RES", response)
          return response.json()
        }
      }).then(data => {
        console.log("DATA", data)
        this.user = data
      })
      },
      default: {
        UserName: ''
      }
    }
  }
}
</script>
