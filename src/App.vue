<template>
  <CalculadoraImc />
</template>

<script>
import CalculadoraImc from "./components/CaluladoraImc.vue";

export default {
  name: "App",
  components: {
    CalculadoraImc: CalculadoraImc,
  },
};
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
body {
  background: #bbded6;
}
</style>
