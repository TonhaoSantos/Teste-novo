<template>
  <div id="app">
    <TesteTela />
  </div>
</template>

<script>
import TesteTela from "./components/TesteTela";

export default {
  name: "App",
  components: {
    TesteTela,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
*,
*::before,
*::after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
