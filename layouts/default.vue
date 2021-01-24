<template>
    <v-app purple darken-4 id="content">
        <header class="header">
            <v-card>
                <v-card-title>
                    CDN Packages
                    <v-spacer></v-spacer>
                    <v-text-field
                        v-model="search"
                        append-icon="mdi-magnify"
                        label="Search"
                        single-line
                        hide-details
                    ></v-text-field>
                </v-card-title>
            </v-card>
        </header>
        <main class="main">
            <v-data-table
                :headers="headers"
                :items="data"
                :items-per-page="10"
                class="elevation-1"
            >
              <template v-slot:item="props">
                <tr>
                  <td data-tooltip="Нажмите на имя пакета">{{ props.item.hits }}</td>
                  <td>                    
                    <modal :currentPackage="currentPackage" 
                           :info="info"
                           :clearInfo="clearInfo"
                    >
                      <template v-slot:activator="{ on }">
                        <div v-on="on"><p @click="getCurrentPackage">{{ props.item.name }}</p></div>
                      </template>
                    </modal>
                  </td>
                  <td data-tooltip="Нажмите на имя пакета">{{ props.item.type }}</td>
                </tr>
              </template> 
            </v-data-table>
        </main>
        <Footer />
    </v-app>
</template>

<script>
import Footer from '@/components/Footer'
import Modal from '@/components/Modal'

export default {
  components: {
    Footer, 
    'modal': Modal
  },
  data: () => ({
      headers: [
            { text: 'hits', value: 'hits' },
            { text: 'name', value: 'name' },
            { text: 'type', value: 'type' }
      ],
      data: [],
      search: '',
      info: [],
      currentPackage: null 
  }),
  methods: {
    getCurrentPackage(e) {
      this.currentPackage = e.toElement.innerText
      this.$axios.get(`http://registry.npmjs.com/-/v1/search?text=${e.toElement.innerText}`)
                 .then(response => {
                    if (Array.isArray(response.data.objects) && response.data.objects.length > 0 ) {
                      var obj = response.data.objects.sort((a,b)=> {
                          parseInt(a.package.version.split(".").join(""))-parseInt(b.package.version.split(".").join(""))
                      })[0]
                      this.info.push({
                        name: 'Version',
                        value: obj.package.version || "0.0.0"
                      },{
                        name: 'Description',
                        value: obj.package.descriprion || "None"
                      },{
                        name: 'Author',
                        value: obj.package.author.name || "None"
                      },{
                        name: 'Author-email',
                        value: obj.package.author.email || "None"
                      },{
                        name: 'Keywords',
                        value: obj.package.keywords.join(", ") || "None"
                      })
                    }
                 })
      },
      clearInfo() {
        this.info = []
      }

  },
  mounted() {
    this.$axios.get("https://data.jsdelivr.com/v1/stats/packages")
               .then(response => {
                    for(let i=0;i<response.data.length;i++) {
                      this.data.push(response.data[i])
                    }
               }).catch(err => alert(err))
    }



}
</script>
