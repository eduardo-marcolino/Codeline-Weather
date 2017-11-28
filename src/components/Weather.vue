<template>
    <div class="weather-single">
        <img :src="weatherIcon( weather.today.weather_state_abbr )" width="30">
        <h1>{{ weather.title }}</h1>
        <p>temperature {{ weather.today.the_temp | round }} </p>
        <p>maximum temperature {{ weather.today.max_temp | round  }} </p>
        <p>minimum temperature {{ weather.today.min_temp | round  }} </p>
    </div>
</template>


<script>
export default {
  props: ['woeid'],
  name: 'weather',
  data () {
    return {
      weather: null
    }
  },
  filters: {
      round(value) {
        return value.toFixed(2)
      }
  },
  methods: {
    loadWeather(woeid) {
      this.$http.get('http://localhost/weather.php?command=location&woeid=' + woeid)
        .then(response => {
          this.weather = response.data
          this.weather.today = response.data.consolidated_weather[0]
        }, error => {
            console.log(error)
        })
    },
    weatherIcon(abbr) {
        return `https://www.metaweather.com/static/img/weather/${abbr}.svg`
    }
  },
  created() {
    this.loadWeather(this.woeid)
  },
}
</script>