<template>
  <div id="app" ref="app">
    <Intro />
  </div>
</template>

<script>
import Intro from './Intro';
export default {
  name: 'app',
  components:{
    Intro
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){
    const app = this.$refs['app'];
  }
}
</script>

<style>
  html,body,#app{
    width: 100%;
    height: 100%;
    margin: 0;
    overflow: hidden;
  }
  body{
    position: relative;
  }
</style>
