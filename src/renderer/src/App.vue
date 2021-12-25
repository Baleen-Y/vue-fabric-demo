<template>
<!-- <img alt="Vue logo" src="./assets/logo.png"> -->
<!-- <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/> -->

<n-message-provider>
  <Canvas />
</n-message-provider>
</template>

<script lang="ts">
// import {
//     Options,
//     Vue
// } from 'vue-class-component';
import Canvas from './components/Canvas.vue';
import { NMessageProvider } from "naive-ui";
// @Options({
//     components: {
//         Canvas,
//         NMessageProvider
//     },
// })
export default {
    components: {
    Canvas,
        NMessageProvider
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
    /* margin-top: 60px; */
}

/* html,
body,
#app {
    height: 100%;
    margin: 0;
    padding: 0;
} */

/* body {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
}
html {
  height: 100%;
  width: 100%;
} */
</style>
