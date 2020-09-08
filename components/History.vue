<template>
  <v-row justify="center">
    <v-col md="4" sm="6" xs="12" class="weather__history text-center" v-if="haveHistory || localStorageHistory">
      <h3>Search history</h3>
      <div
        class="history-item"
        v-for="(item, index) in searchHistory"
        v-bind:key="index"
      >
        <p>{{ item.name }}</p>
        <p>{{ item.temperature }}&#176;</p>
        <div class="remove"
        @click="deleteCity(index)">X</div>
      </div>
      <v-btn color="primary" @click="clearHistory">Clear history</v-btn>
      <v-btn class="localStorageBtn" color="warning" @click="saveHistory">Save history in Local Storage</v-btn>
      <v-btn class="localStorageBtn"
        v-if="localStorageHistory"
        color="error"
        @click="clearLocalStorageHistory"
      >Delete history from Local Storage</v-btn>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class History extends Vue {
  @Prop() readonly haveHistory!: any
  @Prop() readonly searchHistory!: any
  @Prop() readonly localStorageHistory!: boolean

  deleteCity(index: number): void {
    this.$emit('deleteCity', index)
  }

  clearHistory(): void {
    this.$emit('clearHistory')
  }
  saveHistory(): void {
    this.$emit('saveHistory')
  }
  clearLocalStorageHistory(): void {
    this.$emit('clearLocalStorageHistory')
  }
}
</script>