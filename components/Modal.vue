<template>
    <v-dialog v-model="dialog" transition="dialog-top-transition" max-width="600">
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
                Запрошенная информация пока не найдена или не может быть получена
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
                <v-btn @click="close">Закрыть</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props:['currentPackage', 'info', 'clearInfo'],
    data: () => ({
        dialog: false,
        isLoading: true
    }),
    methods: {
        close() {
            this.dialog = false;
            this.clearInfo()
        }
    },
    mounted() {
        this.isLoading = false
    }
}
</script>