<template>
  <div id="app">
    <imagineScience/>
  </div>
</template>

<script>
import imagineScience from './components/imagineScience.vue'

export default {
  name: 'App',
  components: {
    imagineScience
  }
}
</script>

<style>
html {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: url("/bg.gif");
  background-size: cover
}
</style>
