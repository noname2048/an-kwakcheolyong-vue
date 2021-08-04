<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <!--<HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <Kwak />
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Kwak from './components/Kwak.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    Kwak,
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
#canvas {
  display: block;
  width: 100%;
  max-width: 1000px;
  border: 1px solid black;
}
</style>
