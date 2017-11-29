<template>
    <div class="weather-single">
      <div class="card">
        <div class="card-content" v-if="loading">
          <i class="mdi mdi-loading mdi-48px mdi-spin"></i>
        </div>
        <div class="card-content with-link" v-if="!loading" @click="details()">
          <div class="media">
            <div class="media-left">
              <b-tooltip :label="weather.today.weather_state_name">
              <figure class="image is-48x48">
                  <img :src="weatherIcon( weather.today.weather_state_abbr )">                
              </figure>
              </b-tooltip>
            </div>
            <div class="media-content">
              <p class="title is-4">{{ weather.title }}</p>
              <p class="subtitle is-6">Temp: {{ weather.today.the_temp | round }}° - (Max: {{ weather.today.max_temp | round  }}° | Min: {{ weather.today.min_temp | round  }}° ) </p>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>


<script>
export default {
  props: ['woeid'],
  name: 'weather',
  data () {
    return {
      weather: null,
      loading: true
    }
  },
  filters: {
    round (value) {
      return value ? value.toFixed(0) : value
    }
  },
  methods: {
    loadWeather (woeid) {
      this.$http.get(`${this.$weather_api}?command=location&woeid=${woeid}`)
        .then(response => {
          this.weather = response.data
          this.weather.today = response.data.consolidated_weather[0]
          this.loading = false
        }, error => {
          this.$snackbar.open({
            message: `Oops, something went wrong ${error.bodyText}`,
            type: 'is-danger',
            actionText: 'Retry',
            onAction: () => {
              this.loadWeather(woeid)
            }
          })
        })
    },
    weatherIcon (abbr) {
      return `https://www.metaweather.com/static/img/weather/${abbr}.svg`
    },
    details () {
      this.$router.push({name: 'weather', params: {woeid: this.woeid}})
    }
  },
  created () {
    this.loadWeather(this.woeid)
  }
}
</script>

<style scoped>
  .with-link { cursor: pointer; }
</style>