<template>
  <main>
    <Launches/>
  </main>
</template>

<script>
import Launches from './components/Launches.vue'

export default {
  name: 'App',
  components: {
    Launches
  }
}
</script>

<style>
@import './assets/base.css';
@import './assets/style.css';

/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
