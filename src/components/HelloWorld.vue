<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <template v-if="!divisionA">
        <h1>Welcome to Tournament Generator!</h1>
        <v-btn theme="dark" class="mt-3" @click="startTournament" :loading="loading">Create a Tournament</v-btn>
      </template>

      <div v-if="divisionA && divisionB">
        <v-container>
          <v-row>
            <v-col>
              <h1>Division A</h1>
              <v-list>
                <v-list-item
                  v-for="team in divisionA"
                  :key="team.name"
                  :title="team.name"
                ></v-list-item>
              </v-list>
            </v-col>

            <v-col>
              <h1>Division B</h1>
              <v-list>
                <v-list-item
                  v-for="team in divisionB"
                  :key="team.name"
                  :title="team.name"
                ></v-list-item>
              </v-list>
            </v-col>
          </v-row>
        </v-container>

        <v-btn theme="dark" class="mt-3" @click="runDivisionGames" :loading="loading">Run Division Games</v-btn>
      </div>
    </v-responsive>
  </v-container>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      loading: false,
      divisionA: null,
      divisionB: null,
      tournament: null
    }
  },
  methods: {
    startTournament() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + '/create_tournament')
        .then(response => {
          this.loading = false
          this.divisionA = response.data.divisionA
          this.divisionB = response.data.divisionB
          this.tournament = response.data.tournament
        })
    },
    runDivisionGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_division_games/${this.tournament.id}`)
        .then(response => {
          this.loading = false
        })
    }
  }
}
</script>
