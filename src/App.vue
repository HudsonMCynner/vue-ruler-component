<template>
  <div id="app">
    <img src="./assets/logo.png">
    <ruler/>
  </div>
</template>

<script>
import Ruler from './components/Ruler'

export default {
  name: 'App',
  components: {
    Ruler
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
