<template>
  <div id="app">
    <main-view></main-view>
  </div>
</template>
<script>
import MainView from './components/MainView.vue'
export default {
  components: { MainView },
  
}
</script>
<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
#app{
  overflow: hidden;
  height: 100vh;
  background-color: #ecf0f1;
}
</style>