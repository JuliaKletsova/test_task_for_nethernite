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
                            <td>{{ props.item.package.author }}</td>
                            <td @click="print()">{{ props.item }}</td>
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
            { text: 'publisher', value: 'publisher' },
            { text: 'description', value: 'description' }
        ],
        //info: []
    }),
    methods: {
        close() {
            this.dialog = false;
        },
        print(e) {
            console.log(e)
        }
    }
}
</script>

<style>

</style>