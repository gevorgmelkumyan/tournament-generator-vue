<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <template v-if="!divisionA">
        <h1>Welcome to Tournament Generator!</h1>
        <v-btn theme="dark" class="mt-3" @click="startTournament" :loading="loading">Create a Tournament</v-btn>
      </template>

      <v-expand-transition>
        <div v-if="divisionA && divisionB">
          <v-container>
            <v-row>
              <v-col>
                <h1 class="mt-10">Division A</h1>
                <team-list :division="divisionA" />
              </v-col>

              <v-col>
                <h1 class="mt-10">Division B</h1>
                <team-list :division="divisionB" />
              </v-col>
            </v-row>
          </v-container>

          <v-btn v-if="divisionA && divisionB && !divisionGames" theme="dark" class="mt-3" @click="runDivisionGames" :loading="loading">Run Division Games</v-btn>
        </div>
      </v-expand-transition>

      <v-expand-transition>
        <div v-if="divisionGames">
          <h1 class="mt-10">Division A</h1>
          <division-game :rows="divisionGames.divisionA.rows" :columns="divisionGames.divisionA.columns" />

          <h1 class="mt-10">Division B</h1>
          <division-game :rows="divisionGames.divisionB.rows" :columns="divisionGames.divisionB.columns" />

          <v-container>
            <v-row>
              <v-col>
                <h1 class="mt-10">Division A Winners</h1>
                <division-game-winners :winners="divisionGames.divisionA.winners" />
              </v-col>

              <v-col>
                <h1 class="mt-10">Division B Winners</h1>
                <division-game-winners :winners="divisionGames.divisionB.winners" />
              </v-col>
            </v-row>
          </v-container>

          <v-btn v-if="divisionGames && !playoffGames" theme="dark" class="mt-3" @click="runPlayoffGames" :loading="loading">Run Playoffs</v-btn>
        </div>
      </v-expand-transition>

      <v-expand-transition>
        <div v-if="playoffGames">
          <h1 class="mt-10">Playoffs</h1>
          <game :data="playoffGames" />

          <v-btn v-if="playoffGames && !semifinalGames" theme="dark" class="mt-3" @click="runSemifinalGames" :loading="loading">Run Semi-Finals</v-btn>
        </div>
      </v-expand-transition>

      <v-expand-transition>
        <div v-if="semifinalGames">
          <h1 class="mt-10">Semi-Finals</h1>
          <game :data="semifinalGames" />

          <v-btn v-if="semifinalGames && !finalGames" theme="dark" class="mt-3" @click="runFinalGames" :loading="loading">Run Finals</v-btn>
        </div>
      </v-expand-transition>

      <v-expand-transition>
        <div v-if="finalGames">
          <h1 class="mt-10">Finals</h1>
          <game :data="finalGames.finals" />

          <h1 class="mt-10">Results</h1>
          <v-list>
            <v-list-item
              v-for="(row, index) in finalGames.results"
              :key="row.game_id"
              :title="`${index+1}. ${row.team_id}`"
            ></v-list-item>
          </v-list>

          <v-btn theme="dark" class="mt-3" color="red" @click="deleteTournament" :loading="loading">Delete Tournament</v-btn>
        </div>
      </v-expand-transition>

      <v-snackbar
        v-model="error"
        :timeout="2000"
        color="error"
        variant="outlined"
      >
        {{ error }}
      </v-snackbar>
    </v-responsive>
  </v-container>
</template>

<script>
import axios from "axios"
import TeamList from "@/components/TeamList.vue";
import DivisionGame from "@/components/DivisionGame.vue";
import DivisionGameWinners from "@/components/DivisionGameWinners.vue";
import Game from "@/components/Game.vue";

export default {
  components: {Game, DivisionGameWinners, DivisionGame, TeamList},
  data() {
    return {
      loading: false,
      divisionA: null,
      divisionB: null,
      tournament: null,
      divisionGames: null,
      playoffGames: null,
      semifinalGames: null,
      finalGames: null,
      error: null
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
        .catch(error => {
          this.loading = false
          this.error = error.message
        })
    },
    runDivisionGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_division_games/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.divisionGames = response.data
        })
        .catch(error => {
          this.loading = false
          this.error = error.message
        })
    },
    runPlayoffGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_playoffs/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.playoffGames = response.data
        })
        .catch(error => {
          this.loading = false
          this.error = error.message
        })
    },
    runSemifinalGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_semi_finals/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.semifinalGames = response.data
        })
        .catch(error => {
          this.loading = false
          this.error = error.message
        })
    },
    runFinalGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_finals/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.finalGames = response.data
        })
        .catch(error => {
          this.loading = false
          this.error = error.message
        })
    },
    deleteTournament() {
      this.loading = true
      axios.delete(import.meta.env.VITE_API_URL + `/tournaments/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.resetData()
        })
        .catch(error => {
          this.loading = false
          this.error = error.message
        })
    },

    resetData() {
      this.divisionA = null
      this.divisionB = null
      this.divisionGames = null
      this.playoffGames = null
      this.semifinalGames = null
      this.finalGames = null
    }
  }
}
</script>
