<template>
  <v-app>
    <v-row>
      <v-col :cols="9">
        <v-container class="custom-container">
          <h1 class="page-title text-capitalize mb-10">
            {{ this.$route.params.table }}
          </h1>
  
          <v-row>
            <v-col v-for="(value, name, i) in data" :key="i" :cols="3">
              <v-card class="mx-auto" height="130" outlined>
                <v-card-subtitle>{{ name }}</v-card-subtitle>
                <v-divider></v-divider>
                <v-card-text>{{ value !== null || value !== undefined ? value : "Sin registro"}}</v-card-text>
              </v-card>
            </v-col>
          </v-row>

          <v-card class="d-flex flex-column mt-10">
            <v-toolbar flat color="primary" dark>
              <v-toolbar-title>Graficas</v-toolbar-title>
            </v-toolbar>
            <v-tabs right dark background-color="primary">
              <v-tab>Peso para edad</v-tab>
              <v-tab>Estatura para edad</v-tab>
              <v-tab>Peso para estatura</v-tab>
              <v-tab-item>
                <v-img
                  contain
                  class="ma-5"
                  aspect-ratio="1.5"
                  lazy-src="@/assets/loader.jpg"
                  max-width="600"
                  :src="
                    'https://api2.bitmec.com/api/onekdays/graphs/weight_age/' +
                    this.$route.params.id
                  "
                >
                </v-img>
              </v-tab-item>
              <v-tab-item>
                <v-img
                  contain
                  class="ma-5"
                  aspect-ratio="1.5"
                  lazy-src="@/assets/loader.jpg"
                  max-width="600"
                  :src="
                    'https://api2.bitmec.com/api/onekdays/graphs/height_age/' +
                    this.$route.params.id
                  "
                >
                </v-img>
              </v-tab-item>
              <v-tab-item>
                <v-img
                  contain
                  class="ma-5"
                  aspect-ratio="1.5"
                  lazy-src="@/assets/loader.jpg"
                  max-width="600"
                  :src="
                    'https://api2.bitmec.com/api/onekdays/graphs/weight_height/' +
                    this.$route.params.id
                  "
                >
                </v-img>
              </v-tab-item>
            </v-tabs>
          </v-card>
        </v-container>
      </v-col>

      <v-col :cols="3">
        <v-list>
          <v-list-item class="grow">
            <v-list-item-avatar size="70" color="grey darken-3">
              <v-img class="elevation-6" :src="avatar"></v-img>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title class="text-h6 mb-1">
                {{ username }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
        <v-divider></v-divider>
        <v-list dense disabled>
          <v-subheader>Informamcion del usuario</v-subheader>
          <v-list-item-group color="primary">
            <v-list-item v-for="(value, name, i) in patient" :key="i">
              <!-- <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon> -->
              <v-list-item-content v-if="value !== undefined && value !== null">
                <v-list-item-title>{{
                  value == true
                    ? "Masculino"
                    : value == false
                    ? "Femenino"
                    : value
                }}</v-list-item-title>
                <v-list-item-subtitle> {{ name }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import Vue from "vue";
import structure from "./structure";
const axios = require("axios");

export default {
  name: "Detail",
  data() {
    return {
      structure,
      avatar: "",
      username: "",
      data: {},
      patient: {},
    };
  },
  async created() {
    const user_id = this.$route.params.id + "/";
    const from_table = this.$route.params.table + "/";
    
    this.data = await this.getUserData(user_id, from_table);

    if (this.data.patient) {
      this.patient = this.data.patient;
      this.avatar = this.patient.profile_picture;
      this.username = this.patient.first_name + " " + this.patient.last_name;

      Vue.delete(this.data, "patient");
      Vue.delete(this.patient, "id");
      Vue.delete(this.patient, "first_name");
      Vue.delete(this.patient, "last_name");
      Vue.delete(this.patient, "profile_picture");
      Vue.delete(this.patient, "alive");
      Vue.delete(this.patient, "active");
      Vue.delete(this.patient, "timestamp");
      Vue.delete(this.patient, "entity");
    }
  },
  methods: {
    getUserData: async (id, table) => {
      return axios
        .get("https://api2.bitmec.com/api/onekdays/" + table + id)
        .then(({ data }) => {
          return data;
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>

<style src="./Details.scss" lang="scss"/>