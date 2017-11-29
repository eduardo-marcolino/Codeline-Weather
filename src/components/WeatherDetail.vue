<template>

  <div>
    <section class="hero is-primary is-medium">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Weather Around The World
          </h1>
          <h2 class="subtitle" v-if="!loading">
            {{ weather.title }} / {{ weather.parent.title }} Weather Forecast
          </h2>
        </div>
      </div>
    </section>

    <div class="container" v-if="!loading">
      <div class="columns is-multiline">

        <div class="column is-4" v-for="day in weather.consolidated_weather">
          <div class="card">
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <b-tooltip :label="day.weather_state_name">
                  <figure class="image is-48x48">
                      <img :src="weatherIcon( day.weather_state_abbr )">                
                  </figure>
                  </b-tooltip>
                </div>
                <div class="media-content">
                  <p class="title is-4">{{ day.applicable_date | weekname }}</p>
                  <p class="subtitle is-6">Temp: {{ weather.today.the_temp | round }}° - (Max: {{ weather.today.max_temp | round  }}° | Min: {{ weather.today.min_temp | round  }}° ) </p>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'WeatherDetail',
  data () {
    return {
      weather: null,
      loading: true
    }
  },
  methods: {
    loadWeather () {
      let woeid = this.$route.params.woeid
      const loadingComponent = this.$loading.open()
      this.$http.get(`${this.$weather_api}?command=location&woeid=${woeid}`)
        .then(response => {
          this.weather = response.data
          this.weather.today = response.data.consolidated_weather[0]
          this.loading = false
          loadingComponent.close()
        }, error => {
          loadingComponent.close()
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
    }
  },
  filters: {
    round (value) {
      return value ? value.toFixed(0) : value
    },
    weekname (value) {
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let d = new Date(value)
      return days[d.getDay()]
    }
  },
  watch: {
    '$route': 'loadWeather'
  },
  created () {
    this.loadWeather()
  }
}
</script>

<style scoped>
  .hero { margin-bottom: 15px;}
</style>
