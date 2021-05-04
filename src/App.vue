<template>
  <div id="app">
    <h1>Shape of Seinfeld</h1>
    <Graph />
  </div>
</template>

<script>
import Graph from './components/Graph.vue'

export default {
  name: 'App',
  components: {
    Graph
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
}

h1 {
  text-align: left;
  margin: 20px;
}
</style>
