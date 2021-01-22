<template>
   <v-app id="inspire">
    <v-data-table
      :headers="headers"
      :items="data"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-app>
</template>

<script>
export default {
  data: () => ({
      headers: [
            { text: 'hits', value: 'hits' },
            { text: 'name', value: 'name' },
            { text: 'type', value: 'type' }
      ],
      test: [
        {hits: 'sld vksjnflvfb', name: 'fdvv', type: 'dfvkn'}
      ],
      data: []      
  }),
  methods: {
  },
  mounted() {
    this.$axios.get("https://data.jsdelivr.com/v1/stats/packages").then(response => {
        for(let i=0;i<response.data.length;i++) {
          this.data.push(response.data[i])
        }
        console.log(this.data)
    })
  }
}
</script>
