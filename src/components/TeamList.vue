<template>
  <v-list>
    <v-list-item
      v-for="team in division"
      :key="team.name"
      :title="team.name"
    ></v-list-item>
  </v-list>
</template>

<script>
export default {
  props: {
    division: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
