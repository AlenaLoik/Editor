<template>
  <div id="app">
    <h1>Try editor on this text:</h1>
    <PanelControl />
  </div>
</template>

<script>
import PanelControl from "@/components/PanelControl";

export default {
  name: "App",
  components: {
    PanelControl
  }
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  margin-top: 60px;
  padding: 20px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

h1 {
  margin: 20px;
}
</style>
