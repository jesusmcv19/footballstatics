<template>
  <div>
    <v-simple-table
        :search="search">
      <template v-slot:default>
        <thead>
          <tr>
            <th
              v-for="cabecera in cabeceras"
              v-bind:key="cabecera"
              class="m-0 p-0"
            >
              {{ cabecera }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="equipo in equipos" v-bind:key="equipo.team.id">
            <td v-bind:class="{'champions': equipo.description == 'Promotion - Champions League (Group Stage)', 'europa-league' : equipo.description == 'Promotion - Europa League (Group Stage)', 'europa-league-prev' : equipo.description == 'Promotion - Europa Conference League (Qualification)', 'descenso' : equipo.description == 'Relegation'}">{{ equipo.rank }}</td>
            <td>
              <img :src="equipo.team.logo" height="20" />
              {{equipo.team.name}}
            </td>
            <td>{{equipo.all.played}}</td>
            <td>{{equipo.all.win}}</td>
            <td>{{equipo.all.draw}}</td>
            <td>{{equipo.all.lose}}</td>
            <td><span>{{equipo.all.goals.for}}</span> : <span>{{equipo.all.goals.against}}</span></td>
            <td>{{equipo.points}}</td>
            <td>{{equipo.form}}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </div>
</template>
<script>
export default {
  name: "Clasificacion",
  data() {
    return {
      puntos: 0,
      equipos: [],
      cabeceras: ["Pos", "Equipo", "PJ", "V", "E", "D", "G", "Pts", "Forma"],
    };
  },
  computed: {
    search() {
      var axios = require("axios").default;

      var options = {
        method: "GET",
        url: "https://v3.football.api-sports.io/standings",
        params: { league: this.$route.params.idLiga, season: "2021" },
        headers: {
          "x-rapidapi-host": "v3.football.api-sports.io",
          "x-rapidapi-key": "4ea797df57097c4612ae1e9ebb94554d",
        },
      };

      axios(options)
        .then((response) => {
          this.equipos = response.data.response[0].league.standings[0];
          console.log(this.equipos);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style scoped>
.champions {
  background-color: rgb(19, 19, 158);
  color: white;
}

.europa-league {
  background-color:rgb(133, 2, 2);
  color: white;
}

.europa-league-prev {
  background-color:darkgoldenrod;
  color: white;
}

.descenso {
  background-color:red;
  color: white;
}
</style>