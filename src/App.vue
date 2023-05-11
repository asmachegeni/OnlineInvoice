<template>
  <TableProducts/>
 
</template>

<script>

import TableProducts from './components/TableProducts.vue'

export default {
  name: 'App',
  components: {
    TableProducts
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
