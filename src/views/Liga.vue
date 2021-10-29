<template v-slot:extension>
  <v-card>
    <v-toolbar color="primary" dark height="40px">
      <v-tabs v-model="tabs" align-with-title>
        <v-tabs-slider color="white"></v-tabs-slider>
        <v-tab v-for="item in items" :key="item">
          {{ item }}
        </v-tab>
      </v-tabs>
    </v-toolbar>
    <v-tabs-items v-model="tabs">
      <v-tab-item>
        <Clasificacion></Clasificacion>
      </v-tab-item>
    </v-tabs-items>
  </v-card>
</template>
<script>
import Clasificacion from '../components/Clasificacion.vue';
export default {
  components: { Clasificacion },
  name: "Liga",
  data() {
    return {
      tabs: null,
      items: ["Clasificación", "Resultados", "Partidos"],
    };
  },
};
</script>
