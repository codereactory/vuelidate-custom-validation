<template>
  <div id="app">
    <h1>Vuelidate Custom Validation</h1>
    <username-form />
  </div>
</template>

<script>
import UsernameForm from './components/UsernameForm.vue'

export default {
  name: 'app',

  components: {
    UsernameForm,
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
