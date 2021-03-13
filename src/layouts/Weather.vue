<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-page class="flex column" :class="bgClass">

        <div class="col q-pt-lg q-px-md">
          <q-input
            @keyup.enter="getWeatherBySearch"
            v-model="search"
            placeholder="Search"
            dark
            borderless
          >
            <template v-slot:before>
              <q-icon
                @click="getLocation"
                name="my_location"
              />
            </template>

            <template v-slot:append>
              <q-btn
                @click="getWeatherBySearch"
                round
                dense
                flat
                icon="search" />
            </template>
          </q-input>
        </div>

        <template v-if="weatherData">
          <div class="col text-white text-center">
            <div class="text-h4 text-weight-light">
              {{ weatherData.name }}
            </div>
            <div class="text-h6 text-weight-light">
              {{ weatherData.weather[0].description }}
            </div>
            <div class="text-h1 text-weight-thin q-my-lg relative-position">
              <span>{{ Math.round(weatherData.main.temp) }}</span>
              <span class="text-h4 relative-position degree">&deg;C</span>
            </div>
          </div>

          <div class="col text-center">
            <img :src="`http://openweathermap.org/img/wn/${ weatherData.weather[0].icon }@2x.png`" />
            <!-- <img width="30" src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg" alt="Bill" /> -->
          </div>
        </template>

        <template v-else>
          <div class="col column text-center text-white">
            <div class="col text-h2 text-weight-thin">
              Quasar<br>Weather
            </div>
            <q-btn
              @click="getLocation"
              class="col"
              flat>
              <q-icon left size="3em" name="my_location"></q-icon>
              <div>Find my location</div>
            </q-btn>
          </div>
        </template>

        <div class="col skyline"></div>

      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Weather',
  data () {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null
    }
  },
  computed: {
    bgClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith('n')) {
          return 'bg-night'
        }
        else {
          return 'bg-day'
        }
      }
    }
  },
  methods: {
    getLocation() {
      this.$q.loading.show()
      navigator.geolocation.getCurrentPosition(
        position => {
          // console.log('position: ', position)
          this.lat = position.coords.latitude
          this.lon = position.coords.longitude
          this.getWeatherByCoords()
        })
    },
    getWeatherByCoords(){
      this.$q.loading.show()

      // https://api.openweathermap.org/data/2.5/weather?lat=35&lon=139&appid={API key}

      const options = {
        method: 'GET',
	      crossDomain: true,
        url: 'https://community-open-weather-map.p.rapidapi.com/weather',
        params: {
          // q: 'London,uk',
          lat: this.lat,
          lon: this.lon,
          units: 'metric',
          // callback: 'test',
          // id: '2172797',
          lang: 'zh_tw',
          // units: '"metric" or "imperial"',
          // mode: 'xml, html'
        },
        headers: {
          'x-rapidapi-key': '43307ddc22msh7f191e616a3cb85p1b2385jsn24748b2f6800',
          'x-rapidapi-host': 'community-open-weather-map.p.rapidapi.com'
        }
      };

      this.$axios(options).then(response => {
        this.$q.loading.hide()
        try {
          console.log('[getWeatherByCoords] response.data=', response.data)
          this.weatherData = response.data
        } catch (error) {
          console.error(error);
        }
      })
    },
    getWeatherBySearch(){
      this.$q.loading.show()

      const options = {
        method: 'GET',
	      crossDomain: true,
        url: 'https://community-open-weather-map.p.rapidapi.com/weather',
        params: {
          q: this.search,
          // lat: this.lat,
          // lon: this.lon,
          units: 'metric',
          // units: '"metric" or "imperial"',
          // callback: 'test',
          // id: '2172797',
          lang: 'zh_tw',
          // mode: 'xml, html'
        },
        headers: {
          'x-rapidapi-key': '43307ddc22msh7f191e616a3cb85p1b2385jsn24748b2f6800',
          'x-rapidapi-host': 'community-open-weather-map.p.rapidapi.com'
        }
      };

      this.$axios(options).then(response => {
        this.$q.loading.hide()
        try {
          console.log('[getWeatherBySearch] response.data=', response.data)
          this.weatherData = response.data
        } catch (error) {
          console.error(error);
        }
      })
    }
  }
}
</script>

<style lang="sass" scoped>
  .q-page
    background: linear-gradient(to bottom, #136a8a, #267871)
    &.bg-night
      background: linear-gradient(to bottom, #232526, #414345)
    &.bg-day
      background: linear-gradient(to bottom, #00b4db, #0083b0)
  .degree
    top: -44px
  .skyline
    flex: 0 0 100px
    background: url(~assets/skyline.png)
    background-size: contain
    background-position: center bottom
</style>
