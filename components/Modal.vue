<template>
    <v-dialog v-model="dialog"
          transition="dialog-top-transition"
          max-width="600"
        >
        <template v-slot:activator="slotData">
            <slot name="activator" v-bind="slotData"/>
        </template>
            <v-card>
                <v-card-title>
                    <v-spacer></v-spacer>
                    {{ currentPackage }}
                    <v-spacer></v-spacer>
                </v-card-title>
                <v-banner class="text-center" v-if="isLoading || info.length === 0" >
                    Запрошенная Вами информация не может быть получена
                </v-banner>
                <template v-if="info.length !== 0">
                        <v-simple-table
                        fixed-header
                        height="300px"
                    >
                        <template v-slot:default>
                            <thead>
                                <tr>
                                <th></th>
                                <th></th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="(item,i) in info"
                                    :key="i"
                                >
                                    <td class="text-left">{{ item.name }}</td>
                                    <td class="text-left">{{ item.value }}</td>
                                </tr>
                            </tbody>
                        </template>                            
                    </v-simple-table>
                </template>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn @click="close">Close</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
</template>

<script>
export default {
    props:['currentPackage', 'info', 'clearInfo'],
    data: () => ({
        dialog: false,
        headers: [
            { text: 'author', value: 'author' },
            { text: 'version', value: 'version' },
            { text: 'popularity', value: 'popularity' },
            { text: 'description', value: 'description' }
        ],
        isLoading: true
    }),
    methods: {
        close() {
            this.dialog = false;
            this.clearInfo()
        },
        print() {
            console.log(this.info)
        }
    },
    mounted() {
        this.isLoading = false
    }
}
</script>

<style scoped lang="sass">
    

</style>