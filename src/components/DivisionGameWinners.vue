<template>
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
      v-for="row in winners"
      :key="row.team_id"
    >
      <td>{{ row.team_id }}</td>
      <td>{{ row.total }}</td>
    </tr>
    </tbody>
  </v-table>
</template>

<script>
export default {
  props: {
    winners: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
