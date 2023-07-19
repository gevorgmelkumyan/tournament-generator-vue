<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <template v-if="!divisionA">
        <h1>Welcome to Tournament Generator!</h1>
        <v-btn theme="dark" class="mt-3" @click="startTournament" :loading="loading">Create a Tournament</v-btn>
      </template>

      <div v-if="divisionA && divisionB && !divisionGames">
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

      <template v-if="divisionGames && !playoffGames">
        <h1>Division A</h1>
        <v-table theme="dark" density="compact">
          <thead>
          <tr>
            <th class="text-left">
              Teams
            </th>
            <th v-for="column in divisionGames.divisionA.columns" class="text-left">
              {{ column.name }} ({{ column.id }})
            </th>
          </tr>
          </thead>
          <tbody>
          <tr
            v-for="row in divisionGames.divisionA.rows"
            :key="row.game_id"
          >
            <td>{{ row[0].team_id }}</td>
            <td v-for="item in row">{{ item.score }}</td>
          </tr>
          </tbody>
        </v-table>

        <h1 class="mt-10">Division B</h1>
        <v-table theme="dark" density="compact">
          <thead>
          <tr>
            <th class="text-left">
              Teams
            </th>
            <th v-for="column in divisionGames.divisionB.columns" class="text-left">
              {{ column.name }} ({{ column.id }})
            </th>
          </tr>
          </thead>
          <tbody>
          <tr
            v-for="row in divisionGames.divisionB.rows"
            :key="row.game_id"
          >
            <td>{{ row[0].team_id }}</td>
            <td v-for="item in row">{{ item.score }}</td>
          </tr>
          </tbody>
        </v-table>

        <v-container>
          <v-row>
            <v-col>
              <h1>Division A Winners</h1>
              <v-table theme="dark" density="compact">
                <thead>
                <tr>
                  <th class="text-center">
                    Team
                  </th>
                  <th class="text-center">
                    Total Score
                  </th>
                </tr>
                </thead>
                <tbody>
                <tr
                  v-for="row in divisionGames.divisionA.winners"
                  :key="row.team_id"
                >
                  <td>{{ row.team_id }}</td>
                  <td>{{ row.total }}</td>
                </tr>
                </tbody>
              </v-table>
            </v-col>

            <v-col>
              <h1>Division B Winners</h1>
              <v-table theme="dark" density="compact">
                <thead>
                <tr>
                  <th class="text-center">
                    Team
                  </th>
                  <th class="text-center">
                    Total Score
                  </th>
                </tr>
                </thead>
                <tbody>
                <tr
                  v-for="row in divisionGames.divisionB.winners"
                  :key="row.team_id"
                >
                  <td>{{ row.team_id }}</td>
                  <td>{{ row.total }}</td>
                </tr>
                </tbody>
              </v-table>
            </v-col>
          </v-row>
        </v-container>

        <v-btn theme="dark" class="mt-3" @click="runPlayoffGames" :loading="loading">Run Playoffs</v-btn>
      </template>

      <template v-if="playoffGames && !semifinalGames">
        <h1 class="mt-10">Playoffs</h1>
        <v-table theme="dark" density="compact">
          <thead>
          <tr>
            <th class="text-center">
              Game
            </th>
            <th class="text-center">
              Score
            </th>
          </tr>
          </thead>
          <tbody>
          <tr
            v-for="row in playoffGames"
          >
            <td>{{ row[0].team_id }} : {{ row[1].team_id }}</td>
            <td>{{ row[0].score }} : {{ row[1].score }}</td>
          </tr>
          </tbody>
        </v-table>

        <v-btn theme="dark" class="mt-3" @click="runSemifinalGames" :loading="loading">Run Semi-Finals</v-btn>
      </template>

      <template v-if="semifinalGames && !finalGames">
        <h1 class="mt-10">Semi-Finals</h1>
        <v-table theme="dark" density="compact">
          <thead>
          <tr>
            <th class="text-center">
              Game
            </th>
            <th class="text-center">
              Score
            </th>
          </tr>
          </thead>
          <tbody>
          <tr
            v-for="row in semifinalGames"
          >
            <td>{{ row[0].team_id }} : {{ row[1].team_id }}</td>
            <td>{{ row[0].score }} : {{ row[1].score }}</td>
          </tr>
          </tbody>
        </v-table>

        <v-btn theme="dark" class="mt-3" @click="runFinalGames" :loading="loading">Run Finals</v-btn>
      </template>

      <template v-if="finalGames">
        <h1 class="mt-10">Finals</h1>
        <v-table theme="dark" density="compact">
          <thead>
          <tr>
            <th class="text-center">
              Game
            </th>
            <th class="text-center">
              Score
            </th>
          </tr>
          </thead>
          <tbody>
          <tr
            v-for="row in finalGames.finals"
          >
            <td>{{ row[0].team_id }} : {{ row[1].team_id }}</td>
            <td>{{ row[0].score }} : {{ row[1].score }}</td>
          </tr>
          </tbody>
        </v-table>

        <h1>Results</h1>
        <v-list>
          <v-list-item
            v-for="(row, index) in finalGames.results"
            :key="row.game_id"
            :title="`${index+1}. ${row.team_id}`"
          ></v-list-item>
        </v-list>
      </template>
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
      tournament: null,
      divisionGames: null,
      playoffGames: null,
      semifinalGames: null,
      finalGames: null
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
          this.divisionGames = response.data
        })
    },
    runPlayoffGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_playoffs/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.playoffGames = response.data
        })
    },
    runSemifinalGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_semi_finals/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.semifinalGames = response.data
        })
    },
    runFinalGames() {
      this.loading = true
      axios.post(import.meta.env.VITE_API_URL + `/run_finals/${this.tournament.id}`)
        .then(response => {
          this.loading = false
          this.finalGames = response.data
        })
    }
  }
}
</script>
