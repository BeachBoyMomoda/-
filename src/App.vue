<template>
  <div id="app">
    <Search></Search>
  </div>
</template>

<script>
import Search from './components/Search'

export default {
  components: {
    Search
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  height: 100%;
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: absolute;
  width: 1800px;
  top: 35%;
  left: 50%;
  margin-top: -200px;
  margin-left: -900px;
}
</style>
