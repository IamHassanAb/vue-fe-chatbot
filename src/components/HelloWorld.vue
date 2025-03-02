<template>
  <div class="hello">
    <ChatBot />
  </div>
</template>

<script>
import ChatBot from './ChatBot.vue';
export default {
  name: 'HelloWorld',
  components: {
    ChatBot
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  display: flex;
  height: 100vh;
  width: 100vw;
  align-items: center;
  justify-content: center;
}

</style>
