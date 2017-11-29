<template>

  <div>
    <section class="hero is-primary is-medium">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Weather Around The World
          </h1>
          <h2 class="subtitle">
            Popular Cities
          </h2>
        </div>
      </div>
    </section>

    <div class="container">
      <div class="columns is-multiline">
        <weather :woeid="2344116" class="column is-4"></weather>
        <weather :woeid="638242" class="column is-4"></weather>
        <weather :woeid="44418" class="column is-4"></weather>
        <weather :woeid="565346" class="column is-4"></weather>
        <weather :woeid="560743" class="column is-4"></weather>
        <weather :woeid="9807" class="column is-4"></weather>
      </div>
    </div>
  </div>
</template>

<script>

import Weather from './Weather.vue'

export default {
  name: 'Home',
  components: { Weather }
}
</script>

<style scoped>
  .hero { margin-bottom: 15px;}
</style>
