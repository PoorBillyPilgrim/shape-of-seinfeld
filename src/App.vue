<template>
  <div id="app">
    <Header />
    <Graph />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Graph from './components/Graph.vue'

export default {
  name: 'App',
  components: {
    Header,
    Graph
  },
  data() {
    return {
      message: 'hello'
    }
  },
  created() {
    this.sayHello()
  },
  methods: {
    sayHello() {
      console.log(this.message)
    }
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
