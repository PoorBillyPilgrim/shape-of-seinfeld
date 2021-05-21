<template>
  <div id="app">
    <Header />
    <Episode />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Episode from './components/Episode.vue'


export default {
  name: 'App',
  components: {
    Header,
    Episode
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0;
}

.row {
  display: flex;
  width: 100%;
}

.col {
  width: 33%;
}
</style>
