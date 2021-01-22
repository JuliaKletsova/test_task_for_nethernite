<template>
    <v-app dark id="content"> 
        <Header />
            <v-data-table
              :headers="headers"
              :items="data"
              :items-per-page="10"
              class="elevation-1"
            ></v-data-table>
            
        <Footer />
    </v-app>
</template>

<script>
import Footer from '@/components/Footer'

export default {
  components: {
    Footer
  },
  data: () => ({
      headers: [
            { text: 'hits', value: 'hits' },
            { text: 'name', value: 'name' },
            { text: 'type', value: 'type' }
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
    })
  }
}
</script>
