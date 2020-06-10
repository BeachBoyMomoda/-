<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
import Search from './components/Search'
import Result from './components/Result'

export default {
  components: {
    Search,
    Result
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  height: 100%;
  background-image: url('./static/img/bkg1.gif');
  background-attachment: fixed;
  background-position: right;
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
