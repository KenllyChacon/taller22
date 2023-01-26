<template>
  <Covid/>
</template>

<script>

import Covid from "@/components/Covid.vue";
export default {
  name: 'App',
  components: {
    Covid
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}

*{
  background-color: #1B1F27;
}
</style>
