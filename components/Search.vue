<template>
  <v-row class="text-center center">
    <v-col sm="6" xs="12">
      <v-form @submit.prevent="weatherRequest">
        <v-text-field v-model="searchCity" @input="getCity" label="Type city for search.." required></v-text-field>
      </v-form>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class Search extends Vue {
  searchCity = "";

  getCity(value: string): void {
    this.$emit("getCity", this.searchCity);
  }

  weatherRequest(): void {
    this.$emit("cityInput");
    this.searchCity = "";
  }
}
</script>