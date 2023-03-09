<template>
  <CountryList/>
  
</template>

<script>

//import HelloWorld from './components/HelloWorld' 
import CountryList from './components/CountryList' 
export default {
  name: 'App',
  components: {
    //HelloWorld,
    CountryList
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
