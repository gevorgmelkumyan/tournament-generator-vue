<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <h1>Welcome to Tournament Generator!</h1>
      <v-btn theme="dark" class="mt-3" @click="startTournament" :loading="loading">Start a Tournament</v-btn>
    </v-responsive>
  </v-container>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      loading: false
    }
  },
  methods: {
    startTournament() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + '/create_tournament')
        .then(response => {
          this.loading = false
          console.log(response)
        })
    }
  }
}
</script>
