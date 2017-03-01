<template>
  <div id="app">
     <div class="header">
       我是头
     </div>
     <div class="tab-link">
         <router-link to="/goods">qu goods</router-link>
     </div>
     <div class="content">
       我是内容
     </div>

    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
