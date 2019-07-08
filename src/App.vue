<template>
  <div id="app">
    
    <!-- <StartScreen/> -->
    <Q1Screen/>
  </div>
</template>

<script>
// import StartScreen from './components/StartScreen.vue'
import Q1Screen from './components/Q1Screen.vue'

export default {
  name: 'app',
  components: {
    // StartScreen
    Q1Screen
  }
}
</script>

<style lang="scss">

#app {
  margin: 0;
  padding: 0;
}
</style>
