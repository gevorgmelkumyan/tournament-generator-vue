<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <h1>Welcome to Tournament Generator!</h1>
      <v-btn theme="dark" class="mt-3">Start a Tournament</v-btn>
    </v-responsive>
  </v-container>
</template>

<script setup>
import axios from "axios";

let loading = false

function startTournament() {
  axios.post()
}
</script>
