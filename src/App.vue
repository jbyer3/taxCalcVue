<template>
  <div id="app">
    <div id="headers">
      <h1>Consume</h1>
      <img class="headers" src="./assets/logo.png">
      <h1>Prilosec</h1>
    </div>
    
    <h2>Make sure you have enough cash!</h2>
    <InputList />

  </div>
</template>

<script>
import InputList from "./components/InputList.vue";

export default {
  name: "app",
  components: {
    InputList
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#headers {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
</style>
