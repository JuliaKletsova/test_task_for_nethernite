<template>
  <v-app>
    <v-data-table
    :headers="headers"
    :item="data"
    :items-per-page="10"
    class="elevation-1"
  ></v-data-table>
  </v-app>
</template>

<script>
export default {
  data: () => ({
      headers: [
            { text: 'hints', value: 'hints' },
            { text: 'name', value: 'name' },
            { text: 'type', value: 'type' }
      ],
      data: []      
  }),
  mounted() {
    this.$axios.get("https://data.jsdelivr.com/v1/stats/packages").then(response => {
        for(let i=0;i<response.data.length;i++) {
          this.data.push(response.data[i])
        console.log(this.data.length)
        }
        console.log(this.data)
    })
  }
}
</script>
