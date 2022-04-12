<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <FormInput value="abc"/>
  </div>
</template>

<script>
import FormInput from './components/FormInput.vue'

export default {
  name: 'App',
  components: {
    FormInput,
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
</style>
