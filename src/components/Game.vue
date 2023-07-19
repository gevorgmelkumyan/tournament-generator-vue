<template>
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
      v-for="row in data"
    >
      <td>{{ row[0].team_id }} : {{ row[1].team_id }}</td>
      <td>{{ row[0].score }} : {{ row[1].score }}</td>
    </tr>
    </tbody>
  </v-table>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
