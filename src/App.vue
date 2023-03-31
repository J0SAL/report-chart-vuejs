<template>
  <LineChart />
  <!-- <LinePlot /> -->
  <HelloWorld />
</template>

<script>
import LineChart from './components/LineChart'
import HelloWorld from './components/HelloWorld'
// import LinePlot from './components/LinePlot'

export default {
  name: 'App',
  components: {
    LineChart,
    HelloWorld,
    // LinePlot
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
  margin-top: 60px;
}
</style>
