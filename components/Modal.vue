<template>
    <v-dialog v-model="dialog"
          transition="dialog-top-transition"
          max-width="600"
        >
        <template v-slot:activator="slotData">
            <slot name="activator" v-bind="slotData"/>
        </template>
            <v-card>
                <v-card-title @click="print">Пакет {{ currentPackage }}</v-card-title>
                <v-data-table
                    :headers="headers"
                    :items="info"
                    :items-per-page="10"
                    class="elevation-1"
                >
                    <template v-slot:item="props">
                        <tr>
                            <td>{{ props.item.package.author.name }}</td>
                            <td>{{ props.item.package.version}}</td>
                            <td @click="print">{{ props.item.score.detail.popularity.toFixed(4) }}</td>
                            <td>{{ props.item.description }}</td>
                        </tr>
                    </template>
                </v-data-table>
                <v-card-actions>
                    <v-btn @click="close">Close</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
</template>

<script>
export default {
    props:['currentPackage', 'info'],
    data: () => ({
        dialog: false,
        headers: [
            { text: 'author', value: 'author' },
            { text: 'version', value: 'version' },
            { text: 'popularity', value: 'popularity' },
            { text: 'description', value: 'description' }
        ],
        //info: []
    }),
    methods: {
        close() {
            this.dialog = false;
        },
        print() {
            console.log(this.info[0].package.author.name)
        }
    }
}
</script>

<style>

</style>