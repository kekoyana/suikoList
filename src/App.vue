<template>
  <div id="app">
    <HeroTable />
  </div>
</template>

<script>
import HeroTable from './components/HeroTable.vue'

export default {
  name: 'App',
  components: {
    HeroTable,
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
