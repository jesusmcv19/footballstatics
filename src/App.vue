<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-icon large>mdi-soccer</v-icon>
      <v-toolbar-title>FootballStatics</v-toolbar-title>
      <Busqueda></Busqueda>
    </v-app-bar>
    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
      class="mx-auto"
      width="400"
    >
      <v-list>
        <v-subheader color="primary">FootballStatics</v-subheader>
        <v-list-group prepend-icon="mdi-podium">
          <template v-slot:activator>
            <v-list-item-title>Competiciones</v-list-item-title>
          </template>

          <v-list-group no-action sub-group>
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Ligas</v-list-item-title>
              </v-list-item-content>
            </template>
            <v-list-item
              v-bind:to="'/liga/' + liga.league.id"
              v-for="liga in ligas"
              v-bind:key="liga.league.id"
            >
              <v-list-item-title>{{ liga.league.name }}</v-list-item-title>
              <v-list-item-icon>
                <img :src="liga.league.logo" height="20" />
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>

          <v-list-group no-action sub-group>
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Copas</v-list-item-title>
              </v-list-item-content>
            </template>
            <v-list-item
              v-bind:to="'/copa/' + copa.league.id"
              v-for="copa in copas"
              v-bind:key="copa.league.id"
            >
              <v-list-item-title>{{ copa.league.name }}</v-list-item-title>
              <v-list-item-icon>
                <img :src="copa.league.logo" height="20" />
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>
        </v-list-group>
        <v-list-group prepend-icon="mdi-tshirt-crew">
          <template v-slot:activator>
            <v-list-item-title>Equipos</v-list-item-title>
          </template>
        </v-list-group>
      </v-list>
    </v-navigation-drawer>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import Busqueda from "./components/Busqueda";

export default {
  name: "App",
  components: {
    Busqueda,
  },
  data: () => ({
    drawer: false,
    ligas: {},
    copas: {},
  }),

  watch: {
    group() {
      this.drawer = false;
    },
  },
  created() {
    var axios = require("axios").default;

    var options = {
      method: "GET",
      url: "https://api-football-v1.p.rapidapi.com/v3/leagues",
      params: { country: "Spain", type: "cup" },
      headers: {
        "x-rapidapi-host": "api-football-v1.p.rapidapi.com",
        "x-rapidapi-key": "680132f04bmsh73486f8cf447ca0p1c2717jsn09eafe83eaed",
      },
    };

    axios
      .request(options)
      .then((response) => {
        this.copas = response.data.response;
      })
      .catch(function (error) {
        console.error(error);
      });

    options.params.type = "league";

    axios
      .request(options)
      .then((response) => {
        this.ligas = response.data.response.sort(
          (a, b) =>
            a.league.id - b.league.id ||
            a.league.name.localeCompare(b.league.name)
        );
      })
      .catch(function (error) {
        console.error(error);
      });
  },
};
</script>
