<template>
  <div id="app">
    <div id="headers">
      <h1>Sales Tax Calculator</h1>
      <!-- <img class="headers" src="./assets/logo.png">
      <h1>Calculator</h1> -->
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
body {
  background: #dd3e54;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #6be585, #dd3e54);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #6be585, #dd3e54); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
#headers {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
</style>
