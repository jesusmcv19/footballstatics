<template>
  <v-row justify="center" align="center">
    <v-col xl="1" lg="1" md="2" sm="3" class="mr-0 pr-0">
      <v-select
        :items="options"
        item-text="valor"
        label="Que Buscar"
        hide-details="auto"
        dense
        solo
      ></v-select>
    </v-col>
    <v-col xl="2" lg="2" md="3" sm="4" class="ml-0 pl-0">
      <v-autocomplete
        v-model="model"
        :items="items"
        :loading="isLoading"
        :search-input.sync="search"
        dense
        chips
        clearable
        hide-details
        hide-selected
        item-text="name"
        item-value="symbol"
        label="Buscar..."
        solo
      >
        <template v-slot:no-data>
          <v-list-item>
            <v-list-item-title>
              Busca el
              <strong>Equipo</strong>
            </v-list-item-title>
          </v-list-item>
        </template>
        <template v-slot:selection="{ attr, on, item, selected }">
          <v-chip
            v-bind="attr"
            :input-value="selected"
            color="blue-grey"
            class="white--text"
            v-on="on"
          >
            <v-icon left> 
                 <img :src="item.team.logo" height="20" />
            </v-icon>
            <span v-text="item.team.name"></span>
          </v-chip>
        </template>
        <template v-slot:item="{ item }">
          <v-list-item-icon>
            <img :src="item.team.logo" height="20" />
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title v-text="item.team.name"></v-list-item-title>
          </v-list-item-content>
        </template>
      </v-autocomplete>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "Busqueda",
  data: () => ({
    isLoading: false,
    items: [],
    model: null,
    search: null,
    tab: null,
    options: [
      {
        peticion: "leagues",
        valor: "Ligas",
      },
      {
        peticion: "teams",
        valor: "Equipos",
      },
    ],
  }),
  watch: {
    model(val) {
      if (val != null) this.tab = 0;
      else this.tab = null;
    },
    search(val) {
      if (this.items.length > 0) return;

      this.isLoading = true;

      var axios = require("axios").default;

      var options = {
        method: "GET",
        url: "https://v3.football.api-sports.io/teams",
        params: {search : val},
        headers: {
          "x-rapidapi-host": "v3.football.api-sports.io",
          "x-rapidapi-key":
            "4ea797df57097c4612ae1e9ebb94554d",
        },
      };

      axios(options)
        .then((response) => {
          this.items = response.data.response;
          console.log(this.items);
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => (this.isLoading = false));
    },
  },
};
</script>
<style scoped>
.center {
  display: block;
  margin: 0 auto;
}
</style>