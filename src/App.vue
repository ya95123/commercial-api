<template>
  <v-app id="app">
    <v-card class="elevation-0">
      <v-card-title>
        <b>全球商機資訊</b>
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
        :expanded.sync="expanded"
        :loading="loading"
        loading-text="全球商機資訊即將蒐集完成..."
        single-expand
        item-key="number"
        show-expand
        class="light"
      >
        <template v-slot:expanded-item="{ headers, item }">
          <td class="py-3 content" style="color:#000;" :colspan="headers.length">{{ item.content }}</td>
        </template>
      </v-data-table>
    </v-card>
    <div class="text-end px-5 caption">
      資料來源：
      <a href="https://www.trade.gov.tw/World/List.aspx?code=7020&nodeID=45&areaID=4&country=b645Lit5ZyL5aSn6Zm4" style="text-decoration:none;">經濟部國際貿易局 經貿資訊網</a>
      </div>
  </v-app>
</template>

<script>
// * api 內文整理
// 正則去掉所有的html標記
const delHtmlTag = (str) => {
  return str.replace(/<[^>]+>/g, '')
}
const delSpace = (str) => {
  return str.replace(/&nbsp;/g, '')
}
const delDot = (str) => {
  return str.replace(/,/g, ' ')
}
const delT = (str) => {
  return str.replace(/T/g, ' ')
}
const delLine = (str) => {
  return str.replace(/-/g, '/')
}

export default {
  name: 'App',
  data: () => ({
    search: '',
    loading: true,
    expanded: [],
    headers: [
      { text: '項目', align: 'center', width: '5%', value: 'number' },
      { text: '地區', align: 'center', width: '20%', value: 'area' },
      { text: '發布時間', align: 'center', width: '20%', value: 'time' },
      { text: '最新消息', align: 'center', width: '55%', value: 'title' }
    ],
    desserts: []
  }),
  mounted () {
    this.axios.get('https://www.trade.gov.tw/Api/Get/pages?nodeid=45&timeRestrict=true')
      .then(response => {
        console.log(response)
        // 限制最多筆為 400 筆
        const dl = response.data.length - 350
        response.data.splice(350, dl)

        let n = 1
        this.$data.desserts = response.data.map(d => {
          return {
            number: n++,
            area: delDot(d.PageSummary),
            time: delLine(delT(d.PagePublishTime)),
            title: d.PageTitle,
            content: d.PageContent
            //  delSpace(delHtmlTag(d.PageContent))
          }
        })
        // 此為正則 html 標籤
        for (let i = 0; i < 350; i++) {
          // n++
          // this.$data.desserts[i].number = n
          this.$data.desserts[i].content = delSpace(delHtmlTag(this.$data.desserts[i].content))
        }
        // 跑完資料，讓 loading 效果消失
        this.$data.loading = false
      })
      .catch(() => {
        console.log('status:check')
      })
  }
}
</script>
