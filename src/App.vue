<template>
  <div id="app">
    <!-- <Options /> -->
    <HelloWorld/>
  </div>
</template>

<script>
// import ChatBot from './components/ChatBot.vue';
import HelloWorld from './components/HelloWorld.vue';
// import Options from './components/SelectOptions.vue';
// import axios from 'axios';

export default {
  name: 'App',
  components: {
    HelloWorld,
    // Options
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100%;
  display: flex;
}
</style>
