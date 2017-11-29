<template>
  <div id="app">

    <nav class="navbar is-primary">
      <div class="navbar-brand">
         <router-link class="navbar-item" to="/">Weather App</router-link>
        <div class="navbar-burger burger" @click="navOpen = !navOpen">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>

      <div class="navbar-menu" v-bind:class="{'is-active': navOpen}">
        <div class="navbar-start">
        </div>

        <div class="navbar-end">
          <div class="navbar-item">
            <div class="field is-grouped">
              <b-field>
                <b-input v-model="keyword" placeholder="Search..." type="search" icon="magnify"></b-input>
                <p class="control">
                  <button class="button is-primary" @click="search()" :disabled="!keyword">Search</button>
                </p>
              </b-field>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <router-view/>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      navOpen: false,
      keyword: this.$route.params.keyword
    }
  },
  methods: {
    search () {
      this.$router.push({name: 'search', params: {keyword: this.keyword}})
    }
  }
}
</script>

<style>
@import url(https://fonts.googleapis.com/css?family=Lobster);
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.navbar-brand a {
  font-family: 'Lobster', cursive;
  font-size: 24px;
  color: #FFDE57 !important;
}
</style>
