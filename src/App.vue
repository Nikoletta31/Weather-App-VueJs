<template>
  <div id="app">
<div>
  <h1>The weather in...</h1>
</div>
     <Weather />
  </div>
</template>

<script>
// @ is an alias to /src
import Weather from "@/components/Weather.vue";

export default {
  name: "Home",
  components: {
    Weather,
  },
};
</script>

<style lang="scss">

@import "@/assets/styles/weather.scss";

*{
  padding:0;
  margin:0;
  box-sizing: border-box;
}

  h1{
    padding-top:30px;
  color:white;
  font-weight:400;
  }



#app {
  font-family: 'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height:700px;

}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
