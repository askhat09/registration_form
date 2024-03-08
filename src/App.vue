<template>
  <div id="app">
    <RegistrationForm />
  </div>
</template>

<script>
import RegistrationForm from './components/RegistrationForm.vue';

export default {
  name: 'App',
  components: {
    RegistrationForm
  }
}
</script>

<style>
body {
  box-sizing: border-box;
  margin: 0;
  padding: 0;

  background-color: #F5F5F5;
  font-family: 'Montserrat', sans-serif;
}
</style>
