<template>
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
</template>

<script>
export default {
    props: ['filterList'],
    data: () => ({
        search: ''
    }),
    watch: {
        search() {
            console.log('CHANGED!')
            this.filterList(this.search)
        }  
    },
}
</script>