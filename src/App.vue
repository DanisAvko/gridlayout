<template>
    <v-app>
        <Plane></Plane>
    </v-app>
</template>

<script>
    import Vue from 'vue'
    import Vuetify from 'vuetify/lib'
    import Plane from "./components/Plane";


    Vue.use(Vuetify)

    export default {
        name: 'App',

        components: {
            Plane
        },

        data: () => ({
            //
        }),
    };
</script>
