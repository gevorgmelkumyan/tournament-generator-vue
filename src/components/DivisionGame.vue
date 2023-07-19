<template>
  <v-table theme="dark" density="compact">
    <thead>
    <tr>
      <th class="text-left">
        Teams
      </th>
      <th v-for="column in columns" class="text-left">
        {{ column.name }} ({{ column.id }})
      </th>
    </tr>
    </thead>
    <tbody>
    <tr
      v-for="row in rows"
      :key="row.game_id"
    >
      <td>{{ row[0].team_id }}</td>
      <td v-for="item in row">{{ item.score }}</td>
    </tr>
    </tbody>
  </v-table>
</template>

<script>
export default {
  props: {
    columns: {
      type: Array,
      required: true
    },
    rows: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
