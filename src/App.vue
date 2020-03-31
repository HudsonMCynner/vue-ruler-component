<template>
  <div id="app">
    <ruler />
  </div>
</template>

<script>
import Ruler from './components/Ruler.vue'

export default {
  name: 'App',
  components: {
    Ruler
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
  background: #42b983;
}
</style>
