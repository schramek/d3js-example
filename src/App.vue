<template>
  <div id="app">
    <!--<img alt="Vue logo" src="./assets/logo.png">-->
    <SunburstD3></SunburstD3>
    <div v-for="item in list" :key="item">
      <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    </div>
  </div>
</template>

<script>
//import HelloWorld from './components/ExampleD3.vue'
//import CircleD3 from "@/components/CircleD3.vue";
import SunburstD3 from "@/components/SunburstD3.vue";

export default {
  name: 'App',
  components: {
    SunburstD3,
    //CircleD3,
    //HelloWorld
  },
  data() {
    return {
      list: [1,2,3]
    }
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
