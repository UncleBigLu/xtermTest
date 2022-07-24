<template>

  <MyXterm socket_url="ws://127.0.0.1:23333"></MyXterm>
</template>

<script>

import MyXterm from "@/components/MyXterm";

export default {
  name: 'App',
  components: {

    MyXterm
  }
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
