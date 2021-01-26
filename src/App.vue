<template>
  <div id="app">
    <mainview></mainview>
  </div>
</template>

<script>
import mainview from '../viewmodel/main.vue'

export default {
  name: 'app',
  components: {
    mainview
    
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
  
}
</style>
