<template>
  <Options />
  <ChatBot />
</template>

<script>
import ChatBot from './components/ChatBot.vue'
import Options from './components/SelectOptions.vue';
// import axios from 'axios';

export default {
  name: 'App',
  components: {
    ChatBot,
    Options
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
  margin-top: 60px;
}
</style>
