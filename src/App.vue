<template>
  <v-app id="app">
    <v-card>
      <v-card-title>
        Nutrition
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="desserts"
        :search="search"
      ></v-data-table>
    </v-card>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data: () => ({
    search: '',
    headers: [
      { text: '項目', align: 'center', value: 'number' },
      { text: '地區', align: 'center', value: 'area' },
      { text: '發布時間', align: 'center', value: 'time' },
      { text: '最新消息', align: 'center', value: 'title' }
    ],
    desserts: []
  }),
  mounted () {

  }
}
</script>
