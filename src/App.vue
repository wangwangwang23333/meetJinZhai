<template>
  <div id="app" style="background-image: url('http://www.ahjinzhai.gov.cn/_res/images/body-bg.jpg');
  background-size: 100%;
  background-repeat: no-repeat;
  ">
    <Navigate />
    <br>
    <br><br><br>
    <router-view/>
  </div>
</template>

<script>
  import Navigate from '@/components/Navigate.vue';
  export default {
    name: 'App',
    components: {
      Navigate,
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
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
