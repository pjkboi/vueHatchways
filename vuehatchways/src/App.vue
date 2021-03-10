<template>
  <div id="app">
    <FetchData />
  </div>
  
</template>

<script>
import FetchData from './components/fetchData.vue'

export default {
  name: 'App',
  components: {
    FetchData,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Raleway&display=swap');
@font-face {
  font-family: "Raleway";
  src: local("Raleway"),
   url(./fonts/Raleway-Regular.ttf) format("truetype");
}
@font-face {
  font-family: "Raleway-Black";
  src: local("Raleway"),
   url(./fonts/Raleway-Bold.ttf) format("truetype");
}
#app {
  font-family: Raleway, Raleway-Black, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  margin: auto;
  margin-top: 5em;
  width: 80%;
}
body{
  background-color: #edeff2;
  height: 100%;
  display: grid;
}
</style>
