<template>
  <v-container>
    <v-row justify="center" class="text-center" v-if="inputFilled">
      <v-col md="4" sm="6" xs="12" class="weather">
        <div class="weather__visual">
          <h2 class="weather__description">{{ searchResult.current.condition.text }}</h2>
          <div class="weather__icon">
            <img :src="searchResult.current.condition.icon" alt />
          </div>
        </div>
        <div class="wrapper">
          <div class="weather__city-info">
            <h3 class="city-info__name">{{ searchResult.location.name }}</h3>
            <p class="sity-info__date">{{ searchResult.location.localtime.slice(0, 10) }}</p>
          </div>
          <div class="weather__temperature">
            <h3 class="temperature">{{ searchResult.current.temp_c }}&#176;</h3>
          </div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Result extends Vue {
  @Prop() readonly searchResult!: any;
  @Prop() readonly inputFilled!: any;
}
</script>