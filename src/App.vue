<template>
    <v-app>
        <InputCard></InputCard>
        <GridCard></GridCard>
    </v-app>
</template>

<script>
    import Vue from 'vue'
    import Vuetify from 'vuetify/lib'

import GridCard from "./components/GridCard";
import InputCard from "./components/InputCard";
    Vue.use(Vuetify)

    export default {
        name: 'App',

        components: {

            InputCard,
            GridCard
        },

        data: () => ({
            //
        }),
    };
</script>
