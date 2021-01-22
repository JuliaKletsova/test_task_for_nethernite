<template>
    <div class="header">
        <v-responsive
      max-width="400"
      class="mx-auto mb-4"
    >
      <v-text-field
        v-model="benched"
        type="number"
        label="Total Benched"
        min="0"
        max="10"
      ></v-text-field>
    </v-responsive>
    </div>
</template>

<script>
export default {
    data: () => ({
      
    })
}
</script>

<style>

</style>