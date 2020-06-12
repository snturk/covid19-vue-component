<template>
  <div id="app">
    
    <covidComponent></covidComponent>
    
  </div>
</template>

<script>
import covidComponent from './components/covidComponent.vue'

export default {
  components:{
    covidComponent
  },
  data() {
    return {
      
    }
  },
  
  
}
</script>

<style>
body{
  position: absolute;
  height: 100%;
  width: 100%;
  margin: 0;
  background: #d8f0f3;
}
#app {
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  
}


</style>
