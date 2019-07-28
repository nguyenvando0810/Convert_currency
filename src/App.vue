<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Currency msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Currency from './components/Currency.vue'

export default {
  name: 'app',
  components: {
    Currency
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
