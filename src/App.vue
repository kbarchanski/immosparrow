<template>
  <div id="app">
    <img alt="Rick and Morty logo" src="./assets/rick-and-morty.jpg" width="200">
    <FormInput v-model="value"/>
  </div>
</template>

<script>
import FormInput from './components/FormInput.vue'

export default {
  name: 'App',
  components: {
    FormInput,
  },
  data () {
    return {
      value: '',
    }
  },
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
