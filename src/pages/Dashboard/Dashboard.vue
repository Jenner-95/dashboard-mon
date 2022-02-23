<template>
  <v-container fluid>
    <div class="dashboard-page">
      <v-row no-gutters class="d-flex justify-space-between mt-10 mb-6">
        <h1 class="page-title">Monitoreos</h1>
      </v-row>
      <v-row>
        <v-col lg="4" sm="6" md="5" cols="12">
          <v-card class="mx-1 mb-1">
            <v-card-title class="pa-6 pb-3">
              <p>Monitoreos</p>
              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text class="pa-6 pt-0">
              <v-row no-gutters class="pb-5">
                <v-col cols="5" class="my-auto">
                  <span
                    class="font-weight-medium card-dark-grey"
                    style="font-size: 24px"
                    >{{ this.monitoring }}</span
                  >
                </v-col>
                <v-col cols="6">
                  <Trend
                    :data="getRandomDataForTrends()"
                    :gradient="mock.trend.gradient"
                    :height="40"
                    stroke-width="4"
                    smooth
                  />
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col lg="4" sm="6" md="5" cols="12">
          <v-card class="mx-1 mb-1">
            <v-card-title class="pa-6 pb-3">
              <p>Vacunación</p>
              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text class="pa-6 pt-0">
              <v-row no-gutters class="pb-5">
                <v-col cols="5" class="my-auto">
                  <span
                    class="font-weight-medium card-dark-grey"
                    style="font-size: 24px"
                    >{{ this.vaccination }}</span
                  >
                </v-col>
                <v-col cols="6">
                  <Trend
                    :data="getRandomDataForTrends()"
                    :gradient="mock.trend.gradient1"
                    :height="40"
                    stroke-width="4"
                    smooth
                  />
                </v-col>
              </v-row>
            </v-card-text>
          </v-card> </v-col
        ><v-col lg="4" sm="6" md="5" cols="12">
          <v-card class="mx-1 mb-1">
            <v-card-title class="pa-6 pb-3">
              <p>Suplementación</p>
              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text class="pa-6 pt-0">
              <v-row no-gutters class="pb-5">
                <v-col cols="5" class="my-auto">
                  <span
                    class="font-weight-medium card-dark-grey"
                    style="font-size: 24px"
                    >{{ this.suplementation }}</span
                  >
                </v-col>
                <v-col cols="6">
                  <Trend
                    :data="getRandomDataForTrends()"
                    :gradient="mock.trend.gradient2"
                    :height="40"
                    stroke-width="4"
                    smooth
                  />
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>

        <v-col cols="12">
          <v-card class="support-requests mx-1 mb-1">
            <v-card-title class="pa-6 pb-0">
              <p>Monitoreos Recientes</p>
              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text class="pa-0">
              <v-simple-table>
                <template v-slot:default>
                  <thead class="pl-2">
                    <tr>
                      <th class="text-left pa-6">PACIENTE</th>
                      <th class="text-left">ALTURA</th>
                      <th class="text-left">PESO</th>
                      <th class="text-left">MUNICIPIO</th>
                      <th class="text-left">DEPARTAMENTO</th>
                      <th class="text-left">STATUS</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in monitoringList"
                      :key="item.id"
                      @click="onClickRow(item.id, 'monitoring')"
                      link
                    >
                      <td class="pa-6">
                        {{
                          item.patient.first_name + " " + item.patient.last_name
                        }}
                      </td>
                      <td>{{ Number(item.height).toFixed(2) }}</td>
                      <td>{{ Number(item.weight).toFixed(2) }}</td>
                      <td>{{ item.patient.city }}</td>
                      <td>{{ item.patient.state }}</td>
                      <td>
                        <v-chip link color="success" class="ma-2 ml-0">
                          OK
                        </v-chip>
                      </td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12">
          <v-card class="support-requests mx-1 mb-1">
            <v-card-title class="pa-6 pb-0">
              <p>Vacunaciones Recientes</p>
              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text class="pa-0">
              <v-simple-table>
                <template v-slot:default>
                  <thead class="pl-2">
                    <tr>
                      <th class="text-left pa-6">PACIENTE</th>
                      <th class="text-left">FECHA ADMINISTRADA</th>
                      <th class="text-left">MUNICIPIO</th>
                      <th class="text-left">DEPARTAMENTO</th>
                      <th class="text-left">STATUS</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in vaccinationList"
                      :key="item.id"
                      @click="onClickRow(item.id, 'vaccinations')"
                    >
                      <td class="pa-6">
                        {{
                          item.patient.first_name + " " + item.patient.last_name
                        }}
                      </td>
                      <td>{{ `${item.timestamp.substring(0, 10)}` }}</td>
                      <td>{{ item.patient.city }}</td>
                      <td>{{ item.patient.state }}</td>
                      <td>
                        <v-chip link color="success" class="ma-2 ml-0">
                          OK
                        </v-chip>
                      </td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12">
          <v-card class="support-requests mx-1 mb-1">
            <v-card-title class="pa-6 pb-0">
              <p>Suplementos Recientes</p>
              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text class="pa-0">
              <v-simple-table>
                <template v-slot:default>
                  <thead class="pl-2">
                    <tr>
                      <th class="text-left pa-6">PACIENTE</th>
                      <th class="text-left">FECHA ADMINISTRADA</th>
                      <th class="text-left">MUNICIPIO</th>
                      <th class="text-left">DEPARTAMENTO</th>
                      <th class="text-left">STATUS</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in suplementationList"
                      :key="item.id"
                      @click="onClickRow(item.id, 'supplementations')"
                    >
                      <td class="pa-6">
                        {{
                          item.patient.first_name + " " + item.patient.last_name
                        }}
                      </td>
                      <td>{{ `${item.timestamp.substring(0, 10)}` }}</td>
                      <td>{{ item.patient.city }}</td>
                      <td>{{ item.patient.state }}</td>
                      <td>
                        <v-chip link color="success" class="ma-2 ml-0">
                          OK
                        </v-chip>
                      </td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>

<script>
import mock from "./mock";
import Trend from "vuetrend";
const axios = require("axios");

export default {
  name: "Dashboard",
  components: {
    Trend,
  },
  data() {
    return {
      mock,
      monitoring: 0,
      monitoringList: [],
      vaccination: 0,
      vaccinationList: [],
      suplementation: 0,
      suplementationList: [],
    };
  },
  created() {
    axios
      .get("https://api2.bitmec.com/api/onekdays/monitoring/")
      .then(({ data }) => {
        this.monitoring = data.length;
        this.monitoringList = data.slice(data.length - 10);
      })
      .catch((error) => console.log(error));
    axios
      .get("https://api2.bitmec.com/api/onekdays/vaccinations/")
      .then(({ data }) => {
        this.vaccination = data.length;
        this.vaccinationList = data.slice(data.length - 10);
      })
      .catch((error) => console.log(error));
    axios
      .get("https://api2.bitmec.com/api/onekdays/supplementations/")
      .then(({ data }) => {
        this.suplementation = data.length;
        this.suplementationList = data.slice(data.length - 10);
      })
      .catch((error) => console.log(error));
  },
  methods: {
    getRandomDataForTrends() {
      const arr = [];
      for (let i = 0; i < 12; i += 1) {
        arr.push(Math.random().toFixed(1) * 10);
      }
      return arr;
    },
    onClickRow(id, selectedTable) {
      this.$router.push({
        name: "Detail",
        params: { id: id, table: selectedTable },
      });
    },
  },
};
</script>

<style src="./Dashboard.scss" lang="scss" />
