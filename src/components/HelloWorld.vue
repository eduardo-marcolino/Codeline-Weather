<template>
  <div class="hello">
    <weather :woeid="2344116"></weather>
    <weather :woeid="638242"></weather>
    <weather :woeid="44418"></weather>
    <weather :woeid="565346"></weather>
    <weather :woeid="560743"></weather>
    <weather :woeid="9807"></weather>
  </div>
</template>

<script>

import Weather from './Weather.vue'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components: { Weather }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
