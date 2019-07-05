<template>
  <div id="app">
    
    <StartScreen/>
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'

export default {
  name: 'app',
  components: {
    StartScreen
  }
}
</script>

<style lang="scss">

#app {
  background: red;
  color: #000;
}
</style>
