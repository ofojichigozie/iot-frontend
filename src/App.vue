<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
  import Login from './components/Login.vue';
  import Home from './components/Home.vue';
  import Help from './components/Help.vue';

  export default {
    name: 'app',
    components: {
      Login,
      Home,
      Help
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
</style>
