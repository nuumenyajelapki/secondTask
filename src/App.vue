<template>
  <div id="app">
    <h1>Simon Says</h1>
    <gamePanel />
  </div>
</template>

<script>
import gamePanel from './components/gamePanel'

export default {
  name: 'App',
  components: {
    gamePanel
  }
}
</script>

<style lang="sass">
#app 
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #ffffff
  margin-top: 60px


body
  background-color: #050506


</style>
