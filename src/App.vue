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
        <!-- loading
        loading-text="最新商機資訊馬上就要出現啦..." -->
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
    this.axios.get('https://www.trade.gov.tw/Api/Get/pages?nodeid=45&timeRestrict=true')
      .then(response => {
        console.log(response)

        this.$data.desserts = response.data.map(d => {
          return {
            area: d.PageSummary,
            time: d.PagePublishTime,
            title: d.PageTitle
          }
        })

        let n = 0
        for (let i = 0; i <= response.data.length; i++) {
          n++
          this.$data.desserts[i].number = n
        }
      })
      .catch(() => {
        console.log('status:check')
      })
  }
}
</script>
