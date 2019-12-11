<template>
 <v-app>
   <GridCard></GridCard>
 </v-app>
</template>

<script>
import GridCard from "./components/GridCard";
import Vue from 'vue'
import Vuetify from 'vuetify/lib'

Vue.use(Vuetify)

export default {
  name: 'App',

  components: {
    GridCard
  },

  data: () => ({
    //
  }),
};
</script>
