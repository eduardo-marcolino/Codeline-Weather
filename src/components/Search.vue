<template>

  <div>
    <section class="hero is-primary is-medium">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Weather Around The World
          </h1>
          <h2 class="subtitle">
            Search Result For "{{ $route.params.keyword }}"
          </h2>
        </div>
      </div>
    </section>

    <div class="container">
      <div class="columns is-multiline">
        <div v-for="result in results" class="column is-4">
          <weather :woeid="result.woeid"></weather>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import Weather from './Weather.vue'

export default {
  name: 'Search',
  data () {
    return {
      results: []
    }
  },
  components: { Weather },
  methods: {
    doSearch () {
      this.results = []
      const loadingComponent = this.$loading.open()
      let keyword = this.$route.params.keyword
      this.$http.get(`${this.$weather_api}?command=search&keyword=${keyword}`)
        .then(response => {
          this.results = response.data
          loadingComponent.close()
        }, error => {
          loadingComponent.close()
          this.$snackbar.open({
            message: `Oops, something went wrong ${error.bodyText}`,
            type: 'is-danger',
            actionText: 'Retry',
            onAction: () => {
              this.doSearch()
            }
          })
        })
    }
  },
  watch: {
    // call again the method if the route changes
    '$route': 'doSearch'
  },
  created () {
    this.doSearch()
  }
}
</script>

<style scoped>
  .hero { margin-bottom: 15px;}
</style>
