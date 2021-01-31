<template>
  <div id="app">
    <Table></Table>
  </div>
</template>

<script>

import Table from "@/components/Table";

export default {
  name: 'App',
  components: {
    Table
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 200px 20%;
}
</style>
