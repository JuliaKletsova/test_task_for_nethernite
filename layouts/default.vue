<template>
    <v-app purple darken-4>
        <Header :filterList="filterList"/>
        <main>
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
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import Modal from '@/components/Modal'

export default {
  components: {
    Header, 
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
      fullData: [],
      info: [],
      currentPackage: null 
  }),
  methods: {
    initPackages() {
      this.$axios.get("https://data.jsdelivr.com/v1/stats/packages")
               .then(response => {
                    for(let i=0;i<response.data.length;i++) {
                      this.data.push(response.data[i])
                    }
               }).catch(err => alert(err))
    },
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
      },
      filterList(searchText) {
        console.log('filtering ....')
        if(searchText.length) {
          console.log('1111',this.fullData)
          var test = function(_) {return new RegExp(searchText.split("").join(".*") + '.*').test(_.name)}
          this.data = this.fullData.filter(test)
        }
        if(searchText === "") this.initPackages()
      },
      async setFullData() {
        return await this.data
      }
  },
  mounted() {
    this.initPackages()
    this.setFullData().then(response => {this.fullData = response})
  }
}
</script>
