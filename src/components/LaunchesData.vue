<template>
<section>
         <v-btn v-on:click="updateLaunches(); setLoading(); isHidden= !isHidden" rounded color="primary" dark >
             {{ isHidden ? "Masquer les lancements": "Charger les lancements"}}
        </v-btn>
 <v-container v-if="isHidden===true"  class="transparent lighten-5">
     <TrinityRingsSpinner 
          v-if="loading" 
          :animation-duration="1200"
          :size="70"
           color="#1976D2"
     />
    <v-row no-gutters>
      <v-col
        v-for="LaunchData in LaunchesData"
        :key="LaunchData.flight_number"
        cols="12"
        sm="3"
      >

           <v-hover v-slot="{ hover }">
    <v-card
      class="mx-auto"
      color="transparent lighten-6"
      max-width="500"
    >
      <v-img
        v-bind:src="LaunchData.links.mission_patch_small" alt="mission-patch"
        height="100%"
        width="100%"
      >
        <v-expand-transition v-if="LaunchData.details">
          <div
            v-if="hover"
            class="d-flex transition-fast-in-fast-out light-blue darken-2 v-card--reveal text-p white--text"
            style="height: 100%;"
          >
            {{LaunchData.details}}
          </div>
        </v-expand-transition>
      </v-img>
      <v-card-text
        class="pt-6"
        style="position: relative;"
      >
        <div class="font-weight-light white--text text-h6 mb-2">
          Fusée : {{LaunchData.rocket.rocket_name}}
        </div>
        <div class="font-weight-light white--text text-h6 mb-2">
          Date : {{LaunchData.launch_year}}
        </div>
        <h3 class="text-h6 font-weight-bold blue--text mb-2">
          Mission : {{LaunchData.mission_name}}
        </h3>
        <div class="font-weight-light white--text text-h7 mb-2">
        Site de Lancement : {{LaunchData.launch_site.site_name_long}}
        </div>
      </v-card-text>
    </v-card>
  </v-hover>
  </v-col>
    </v-row>
  </v-container>
</section>
</template>

<script>
import axios from 'axios';
import {TrinityRingsSpinner} from 'epic-spinners'

export default {
  name: 'LaunchesData',
  props: {
    url: String,
  },
   components: {
    TrinityRingsSpinner
  },
  data() {
    return {
      LaunchesData: {},
      isHidden: false,
      loading: true,
    };
  },
  methods: {
    // Requête Axios récupérant les infos du prochain lancement
    updateLaunches() {
      axios
        .get(this.url)
        .then((response) => { this.LaunchesData = response.data; });
    },
    setLoading() {
      axios
        .get(this.url)
        .then(() => { this.loading = false })
        .catch(error => error.response.data);
    },
  },
};
</script>

<style scoped>
    .mx-auto {
      padding: 2em;
      margin:2em;
      height: fit-content;
      text-shadow: 1px 1px 2px black;
    }

     .trinity-rings-spinner {
        margin-top: 1em;
        margin-bottom: 1em;
        margin-left: auto;
        margin-right: auto;
        text-align: center;
      }

</style>
