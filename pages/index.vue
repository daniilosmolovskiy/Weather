<template>
  <v-container class="home">
    <v-row class="text-center center">
      <v-col sm="6" xs="12">
        <h1>{{ text }}</h1>
      </v-col>
    </v-row>
    <Search v-model="this.searchCity" @cityInput="weatherRequest" @getCity="getCity" />
    <Result :searchResult="searchResult" :inputFilled="inputFilled" />
    <History
      :haveHistory="haveHistory"
      :searchHistory="searchHistory"
      :localStorageHistory="localStorageHistory"
      @deleteCity="deleteCity"
      @clearHistory="clearHistory"
      @saveHistory="saveHistory"
      @clearLocalStorageHistory="clearLocalStorageHistory"
    />
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import axios from 'axios'
import VueSweetalert2 from 'vue-sweetalert2'
Vue.use(VueSweetalert2)

// @ is an alias to /src
import Search from '~/components/Search.vue'
import Result from '~/components/Result.vue'
import History from '~/components/History.vue'

interface HistoryItem {
  name: string
  temperature: string
}

@Component({
  components: { Search, Result, History },
})
export default class Home extends Vue {
  text = 'Weather App'
  searchCity = ''
  searchResult: any = {}
  searchHistory: Array<null | HistoryItem> = []
  inputFilled = false
  haveHistory = false
  localStorageHistory = false

  mounted() {
    if (localStorage.history) {
      this.searchHistory = JSON.parse(localStorage.history)
      this.localStorageHistory = true
    }
  }

  getCity(value: string): void {
    this.searchCity = value
  }

  weatherRequest(): void {
    axios
      .get(
        `https://api.weatherapi.com/v1/current.json?key=6d981d7a429c476397681704202708&q=${this.searchCity}`
      )
      .then((response) => (this.searchResult = response.data))
      .then(() => {
        this.searchCity = ''
        this.searchHistory.push({
          name: this.searchResult.location.name,
          temperature: this.searchResult.current.temp_c,
        })
        this.haveHistory = true
        this.inputFilled = true
      })
      .catch((error) => {
        console.log(error)
        this.alertDisplay()
      })
  }

  alertDisplay(): void {
    this.$swal({
      title: 'Error!',
      text: 'Incorrect city name',
      icon: 'error',
      confirmButtonText: 'Got it',
    })
  }
  deleteCity(cityIndex: any): void {
    this.searchHistory.splice(cityIndex, 1)
    if (this.searchHistory.length === 0) {
      this.haveHistory = false
    }
  }

  clearHistory(): void {
    this.searchHistory = []
    this.haveHistory = false
  }
  saveHistory(): void {
    localStorage.history = JSON.stringify(this.searchHistory)
    this.localStorageHistory = true
  }
  clearLocalStorageHistory(): void {
    localStorage.history = []
    this.localStorageHistory = false
  }
}
</script>

<style lang="scss">
main {
  background: url('../assets/Background.jpg') center center no-repeat;
  background-size: cover;
  background-position: bottom;
  color: #fff;
}
.v-application p {
  margin: 0;
}
p {
  margin: 0;
}
h1 {
  font-weight: 300;
}
.weather {
  border-radius: 30px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  &__description {
    font-size: 2rem;
    margin-bottom: 10px;
  }
  &__visual {
    padding: 10px;
    width: 100%;
    border-radius: 4px;
    background: rgba($color: #fff, $alpha: 0.2);
    margin-bottom: 20px;
  }
  &__city-info,
  &__temperature {
    width: 50%;
    padding: 20px;
    background: rgba($color: #fff, $alpha: 0.2);
    height: 100px;
    margin: 0 10px;
    border-radius: 4px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  &__history {
    margin-top: 30px;
    width: 100%;
  }
}
.history-item {
  position: relative;
  display: flex;
  width: 100%;
  justify-content: space-between;
  padding: 10px;
  background: rgba($color: #fff, $alpha: 0.2);
  border-radius: 4px;
  margin-bottom: 10px;
  .remove {
    width: 5px;
    height: 5px;
    color: red;
    position: absolute;
    right: -10px;
    top: 10px;
    &:hover {
      cursor: pointer;
    }
  }
}
.center {
  display: flex;
  justify-content: center;
  text-align: center;
}
.wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 100%;
}
button {
  max-width: 290px;
  margin: 5px;
}
.localStorageBtn {
  font-size: 10px !important;
}
.swal2-header,
.swal2-content,
.swal2-confirm {
  font-family: 'Roboto', sans-serif;
}
</style>
