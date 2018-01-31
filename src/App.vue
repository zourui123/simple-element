<template>
  <div id="app">
    <leftNav></leftNav>
    <div class="main left">
      <router-view/>
    </div>
  </div>
</template>

<script>
import leftNav from './components/leftNav'
export default {
  name: 'App',
  components:{
     leftNav
  }
}
</script>

<style lang="scss">
@import './style/common';
@import "src/style/mixin";
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.main{
   background: #EFF2F7;
   @include width_height(95%,100vh);
   overflow: hidden;
}
</style>
